# Veri-Tabanl--Chat-Bot
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;
using System.Data.SqlClient;
namespace sql_tera
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }
        SqlConnection baglanti = new SqlConnection(@"Data Source=DESKTOP-F7Q4FDK\SQLEXPRESS;Initial Catalog=chatbot;Integrated Security=True");
        String s;
        private void Form1_Load(object sender, EventArgs e)
        {
            
        }
        private void textBox1_KeyDown(object sender, KeyEventArgs e)
        {
            if (e.KeyCode == Keys.Enter)
            {
                try
                {
                    cvplbl.Text = "nasıl?";
                    listBox1.Items.Clear();
                    baglanti.Open();
                SqlCommand komut = new SqlCommand("select * from veri", baglanti);
                SqlDataReader oku = komut.ExecuteReader();
                    s = textBox1.Text.ToLower().Replace(" ", "");
                    while (oku.Read())
                    {
                        if (s.Contains(oku[1].ToString()))
                        {
                            cvplbl.Text = "";
                            listBox1.Items.Add("Tera:"+oku[2].ToString());
                        }
                    }
                
                
                baglanti.Close();




                if (cvplbl.Text == "nasıl?")
                {
                    textBox2.Visible = true;
                    button2.Visible = true;
                } 
                    else textBox1.Text = "";

                }
                catch (Exception)
                { 
                   
                    MessageBox.Show("Veri Tabanı Baglanti Hatası");
                }
            }
        }
        private void button1_Click(object sender, EventArgs e)
        {
            Form2 f = new Form2();
            f.Show();
            
        }
        private void button2_Click(object sender, EventArgs e)
        {
            textBox1.Text= textBox1.Text.Replace(" ", "");
            baglanti.Open();
            SqlCommand komut = new SqlCommand("insert into veri(soru,cevap) Values(@mno,@adsoyad)", baglanti);
            komut.Parameters.AddWithValue("@mno", textBox1.Text);
            komut.Parameters.AddWithValue("@adsoyad", textBox2.Text);
            komut.ExecuteNonQuery();
            baglanti.Close();
            textBox2.Visible = false;
            button2.Visible = false;
            textBox1.Text = "";
            textBox2.Text = "";
         }
        
    }
}
