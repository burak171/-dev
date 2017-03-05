using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.IO;
using System.Net;
using System.Collections;
using System.Xml;
using System.Xml.Linq;
namespace ödev
{
    class Program
    {
        static Dictionary<String, Int32> kelimeHash = new Dictionary<String, Int32>();
        static void dosyadanOku(string dosyaIsmi)
        {
            StreamReader dosyaOku;
            string satir;

            Int32 countKelime = 0;
            dosyaOku = File.OpenText("C:\\Users\\BurakEvci\\Desktop\\Yeni klasör\\mobydick.txt");
            while ((satir = dosyaOku.ReadLine()) != null)
            {
                List<String> kelimeSplit = new List<String>();
                satir = satir.ToLower();
                char[] charSeparators = new char[] { ' ', '.', ',', '!', '?', ':', ';', '-', '#', '*', '[', ']', ')', '(', '\'', '-', 'ı', '"' };
                kelimeSplit = satir.Split(charSeparators, StringSplitOptions.RemoveEmptyEntries).ToList();

                foreach (String kelime in kelimeSplit)
                {
                    countKelime = 0;
                    if (kelimeHash.ContainsKey(kelime))
                    {
                        countKelime = kelimeHash[kelime];
                        countKelime = countKelime + 1;
                        kelimeHash[kelime] = countKelime++;
                    }
                    else
                    {
                        kelimeHash.Add(kelime, 1);
                    }
                }
            }
            dosyaOku.Close();
            foreach (KeyValuePair<String, Int32> entry in kelimeHash)
            {
                Console.WriteLine("Kelime: " + entry.Key + " Sayısı: " + entry.Value);

            }
            var items = kelimeHash.OrderByDescending(i => i.Value);

            XmlTextWriter top10 = new XmlTextWriter("top10words.xml", Encoding.UTF8);
            top10.Formatting = Formatting.Indented;
            top10.WriteStartDocument();
            top10.WriteStartElement("words");
            Int32 count = 0;
            foreach (var item in items)
            {
                if (count >= 10)
                    break;
                Console.WriteLine("{0}: {1}", item.Key, item.Value);
                top10.WriteElementString("word text", item.Key); top10.WriteElementString("count", item.Value.ToString());
                count = count + 1;                
            }              
            top10.WriteEndElement();
            top10.Close();            
        }
        static void Main(string[] args)
        {
            dosyadanOku("");
            Console.ReadKey();
        }    
    }
}
