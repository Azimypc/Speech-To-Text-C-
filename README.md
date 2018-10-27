using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Speech.Recognition;
using System.IO.Ports;
namespace SpeekToTest
{
    class Program
    {
        static void Main(string[] args)
        {
            // www.azimypc.de//
            //azimypc@web.de//
            //00491608739844//

            //instance of SpeechRecognitionEngine Class

            SpeechRecognitionEngine speech = new SpeechRecognitionEngine(new System.Globalization.CultureInfo("en"));

            speech.SetInputToDefaultAudioDevice();
            speech.LoadGrammar(new DictationGrammar());
            speech.RecognizeAsync(RecognizeMode.Multiple);
            speech.SpeechRecognized += Speech_SpeechRecognized;

            // Loop While For control cloce App...

            while (true)
            {
                Console.ReadKey();
            }
        }


        // Manege Event SpeechRecognizedEventArgs whit SpeechRecognizedEventArgs `e´

        private static void Speech_SpeechRecognized(object sender, SpeechRecognizedEventArgs e)
        {
            // Result and see text after speech to CMD

            Console.WriteLine(e.Result.Text);

            // code for Connection to the Arduino Board
            // if you don,t have a Arduino board.make Deaktiv alle the code for connection to board and Run App.

            SerialPort port = new SerialPort();
            port.BaudRate = 9600;
            port.PortName = "COM5";
            port.Open();

            // Send command to board

            if (e.Result.Text == "A")
            {
                port.Write("0");
            }
            else if (e.Result.Text == "B")
            {
                port.Write("1");
            }

            // Port in ending must close!!!

            port.Close();
        }
    }
}
