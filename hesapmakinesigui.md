package hesapmakinesi;

import java.awt.BorderLayout;

import java.awt.Button;

import java.awt.Color;

import java.awt.Font;

import java.awt.Frame;

import java.awt.GridLayout;

import java.awt.Label;

import java.awt.Panel;

import java.awt.event.ActionEvent;

import java.awt.event.ActionListener;

import java.awt.event.WindowAdapter;

import java.awt.event.WindowEvent;

public class HesapMakinesi extends Frame implements ActionListener {

    Label     display;

    Button    onOff;

    Button[]  tuslar;

    Frame     parent;

    long oncekiSayi = 0;

    char operatie = '=';

    boolean yeniGirdi=true;

    Font bigFont = new Font("Arial",Font.PLAIN,24);

   

    public static void main(String[] arg) {

        new HesapMakinesi().setVisible(true);

    }

   

    public HesapMakinesi() {

        super("Hesap makinesi"); 

        olusturGUI();

        startFlashing();

    }

   

    public void olusturGUI() {

        parent=this;

        display = new Label(" ",Label.RIGHT);

        display.setBackground(Color.white);

        display.setFont(bigFont);

       

        onOff = new Button("On");

        onOff.addActionListener(this);

        onOff.setFont(bigFont);

       

        Panel tusPaneli = new Panel(); 

        tusPaneli.setLayout(new GridLayout(4,4));

        String[] isaretler = {"9","8","7", "/",

                           "6","5","4", "*",

                           "3","2","1", "-",

                           "0","C","=", "+" };

        tuslar = new Button[16];

       

      

        for (int b=0; b<16; b++) {

            tuslar[b]=new Button(isaretler[b]);

            tuslar[b].setFont(bigFont);

            tuslar[b].addActionListener(this);

            tusPaneli.add(tuslar[b]);

            }

           

          

              

        this.add(display,BorderLayout.NORTH);

        this.add(tusPaneli,BorderLayout.CENTER);

        this.add(onOff,BorderLayout.SOUTH);

       
        this.setSize(250,250);

       

        this.addWindowListener(new WindowAdapter() {

           

                    public void windowClosing(WindowEvent we){

                        System.exit(0);

                        }

                    }

                );

         

         

    }

   

    public void actionPerformed(ActionEvent evt) {

    

        

        if ( onOff==evt.getSource() ) {

            doOnOff();

            return;

            }

        if (onOff.getLabel().equals("On")) 

                                                     

            return;

       

        char input = evt.getActionCommand().charAt(0); 

        System.out.println("input:"+input);

       

        if (input>='0' & input<='9' ) {

             if (display.equals("0") || yeniGirdi)

                display.setText(input+"");

             else

                display.setText(display.getText()+input);

             return;

            }

 

        if (input=='C') {

            oncekiSayi=0;

            operatie='=';

            yeniGirdi=true;

            display.setText("0");

            return;

            }  

       

        String tekst="0"+display.getText().trim();

        long sayi = Long.parseLong(tekst);

 

        hesapla(input,sayi);

        display.setText(oncekiSayi+"");

        }

     

    public void hesapla(char input, long sayi) {

        System.out.println("hesapla:"+input+"|"+sayi);

        switch (operatie) {

            case '=' : oncekiSayi= sayi; break;

            case '+' : oncekiSayi+=sayi; break;

            case '-' : oncekiSayi-=sayi; break;

            case '*' : oncekiSayi*=sayi; break;

            case '/' : oncekiSayi/=sayi; break;

            }          

        operatie=input;

        yeniGirdi=true;

        }

   

      

    public void doOnOff() {

        yeniGirdi=true;

        if ( onOff.getLabel().equals("On") ) {

            onOff.setLabel("Off");

            display.setBackground(Color.white);

            display.setText("0");

            return;

            }

        onOff.setLabel("On");

        display.setText(" ");

        startFlashing();

        }

 

public void startFlashing() {

    Runnable flash = new Runnable() {

        public void run() {

            boolean white=true;

            while(onOff.getLabel().equals("On")) {

                if (white) display.setBackground(Color.white);

                       else display.setBackground(Color.white);

                white=!white;

                try { Thread.sleep(600); } catch (Exception ex) { }  

                }                   

            } 

        }; 

      new Thread(flash).start();

     }

   

    }
