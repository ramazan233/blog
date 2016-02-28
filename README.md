# blog
using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.Web.UI;
using System.Data;
using System.Data.SqlClient;

using System.Web.UI.WebControls;

namespace siteblog.admin
{
    public partial class kategoriler : System.Web.UI.Page
    {
        sqlbaglanti baglan = new sqlbaglanti();
        SqlConnection baglan1 = new SqlConnection();
        string kategoriId = "";
        string islem = "";

        protected void Page_Load(object sender, EventArgs e)
        {
            kategoriId = Request.QueryString["kategoriID"];
            islem = Request.QueryString["islem"];

            if (islem == "sil")
            {

                SqlCommand cmdsil = new SqlCommand("delete from kategori where kategoriID='"+kategoriId+"'", baglan.baglan());
                cmdsil.ExecuteNonQuery();

            }




            if (Page.IsPostBack == false)
            {

                panel_kategoriekle.Visible = false;
            }


            //kategorileri dataliste çekme
            SqlCommand cmdkgetir = new SqlCommand("select * from kategori", baglan.baglan());
            SqlDataReader drkgetir = cmdkgetir.ExecuteReader();
            dt_kategori_duzenle.DataSource = drkgetir;
            dt_kategori_duzenle.DataBind();   
        }

        protected void Button2_Click(object sender, EventArgs e)
        {
            panel_kategoriekle.Visible = true;
        }

        protected void btn_kategoriekle_eksi_Click(object sender, EventArgs e)
        {
            panel_kategoriekle.Visible = false;
        }

        protected void btn_kategoriekle_Click(object sender, EventArgs e)
        {
            if (flup_resim.HasFile)
            {

                flup_resim.SaveAs(Server.MapPath("/kategori_resim/" + flup_resim.FileName));
                SqlCommand cmdkategoriekle = new SqlCommand("insert into kategori(kategoriAdi,kategoriSira,kategoriAdet,kategoriResim) Values ('"+txtbx_adi.Text+"','"+txtbx_sira.Text+"','"+txtbx_adet.Text+"','/kategori_resim/"+flup_resim.FileName+"')", baglan.baglan());
                cmdkategoriekle.ExecuteNonQuery();
                Response.Redirect("kategoriler.aspx");
            }
            else
            {
                btn_kategoriekle.Text = "RESİM EKLE !";
            }
        }

        protected void btn_kategoriduzenle_arti_Click(object sender, EventArgs e)
        {
            pnlkategoriduzenle.Visible = true;
        }

    

        protected void btn_kategoriduzenle_eksi_Click(object sender, EventArgs e)
        {
            pnlkategoriduzenle.Visible = false;
        }

        protected void dt_kategori_duzenle_SelectedIndexChanged(object sender, EventArgs e)
        {

        }
    }
}
