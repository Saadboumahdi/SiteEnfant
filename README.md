# SiteEnfant
first code
using GestionHotel.Models;
using System;
using System.Collections.Generic;
using System.Data.Entity;
using System.Linq;
using System.Web;
using System.Web.Mvc;

namespace GestionHotel.Controllers
{
    public class PersonneController : Controller
    {
         private GestionHotelEntities db = new GestionHotelEntities();
        // GET: Personne
        public ActionResult teste()
        {
            /// lister comments li kit3wdo bzzf 

            // List<string>  comments = (from c in db.commentaire
            //                 where c.comment.Contains("wooo")
            //                  group c by c.comment into g
            //                where g.Count() >= 2
            //               select g.Key).Take(6).ToList();
            //ViewBag.comments = comments;







            //List<string> users = db.personne.Select(u => u.first_name).ToList();
            //var users = db.personne.Select(u => new { u.first_name, u.last_name }).ToList();
            /* var users =  (from p in db.personne

                             select  p.first_name).ToList();
             var users1 = (from p in db.personne

                          select  p.last_name ).ToList();

             string userList = string.Join("\n", users);
             string userList1 = string.Join("\n", users1);*/

            //Display the results
            /* ViewBag.numUsers = "Number of users: " + numUsers.ToString();
             ViewBag.userList = users;
             ViewBag.userList1 = users1;*/
            ///count reseravtions
            var threeDaysBack = (from r in db.reservation
                                 select DbFunctions.AddDays(DbFunctions.AddDays(DateTime.Now, -30), 0)).FirstOrDefault();
            //ViewBag.threed = threeDaysBack;
            var currentDate = (from r in db.reservation
                               select DbFunctions.AddDays(DateTime.Now, 0)).FirstOrDefault();
            // ViewBag.curr = currentDate;


            //int numUsers = db.personne.Count();

            var countreseravtion = (from r in db.reservation
                                    join ro in db.rooms on r.id_room equals ro.id_room
                                    join h in db.hotel on ro.id_hotel equals h.id_hotel
                                    where r.date >= threeDaysBack
                                      && r.date <= currentDate && h.id_personne == 2
                                    select r).Count();
            ViewBag.countreseravtion = countreseravtion;


            ///count comment pos
            var commentpos = (from c in db.commentaire
                              join h in db.hotel on c.id_hotel equals h.id_hotel
                              where c.value == 1 && h.id_personne == 2
                              select c).Count();
            ViewBag.commentpos = commentpos;

            ///count comments negativ
            var commentnega = (from c in db.commentaire
                               join h in db.hotel on c.id_hotel equals h.id_hotel
                               where c.value == 0 && h.id_personne == 2
                               select c).Count();
            ViewBag.commentnega = commentnega;

            /// lister comments li kit3wdo bzzf 

            // List<string>  comments = (from c in db.commentaire
            //                 where c.comment.Contains("wooo")
            //                  group c by c.comment into g
            //                where g.Count() >= 2
            //               select g.Key).Take(6).ToList();
            //ViewBag.comments = comments;

            ///count client
            var query = db.personne.Where(x => x.typeuser.Equals("client")).Count();
            ViewBag.client = query;





            //List<string> users = db.personne.Select(u => u.first_name).ToList();
            //var users = db.personne.Select(u => new { u.first_name, u.last_name }).ToList();
            /* var users =  (from p in db.personne

                             select  p.first_name).ToList();
             var users1 = (from p in db.personne

                          select  p.last_name ).ToList();

             string userList = string.Join("\n", users);
             string userList1 = string.Join("\n", users1);*/

            //Display the results
            /* ViewBag.numUsers = "Number of users: " + numUsers.ToString();
             ViewBag.userList = users;
             ViewBag.userList1 = users1;*/

            return View();
        }
        public ActionResult Index()
        {
            int idddd = 1;
            string typeuser = (string)HttpContext.Session["typeuser"];
            string idd = (string)HttpContext.Session["id"];
            int id = Convert.ToInt32(idd);
            @ViewBag.ift = id;
            @ViewBag.idddd = idddd;
            //List<int> hotelids;
            var hotelids = (from h in db.hotel
                       where h.id_personne == id
                       select h.id_hotel).ToList();
            ViewBag.hotelids = hotelids;
            var images = (from p in db.photo join h in db.hotel on p.id_hotel equals h.id_hotel
                          where h.id_personne==id && p.type_photo=="principal" select p.nom_photo).ToList();
            ViewBag.images = images;
            var hotelnom = (from h in db.hotel
                            where h.id_personne == id
                            select h.name_hotel).ToList();
            ViewBag.hotelnom = hotelnom;
            if (id != 0) {
                if (typeuser == "admin")
                {
                    //hotel of user
                    /*  var personnes = db.personne.Where((elt => elt.typeuser == "admin")).ToList();
                      return View(personnes);*/

                    ///count client
                    //var query = db.personne.Where(x => x.typeuser.Equals("client")).Count();
                    var client = (from r in db.reservation
                                    join ro in db.rooms on r.id_room equals ro.id_room
                                    join h in db.hotel on ro.id_hotel equals h.id_hotel
                                    where h.id_personne == id
                                    select r).Count();
            ViewBag.client = client;
            ///count reseravtions
            var threeDaysBack = (from r in db.reservation
                                 select DbFunctions.AddDays(DbFunctions.AddDays(DateTime.Now, -30), 0)).FirstOrDefault();
            //ViewBag.threed = threeDaysBack;
            var currentDate = (from r in db.reservation
                               select DbFunctions.AddDays(DateTime.Now, 0)).FirstOrDefault();
            // ViewBag.curr = currentDate;


            //int numUsers = db.personne.Count();

            var countreseravtion = (from r in db.reservation
                                    join ro in db.rooms on r.id_room equals ro.id_room
                                    join h in db.hotel on ro.id_hotel equals h.id_hotel
                                    where r.date >= threeDaysBack
                                      && r.date <= currentDate && h.id_personne == id
                                    select r).Count();
            ViewBag.countreseravtion = countreseravtion;
            var pourcentageclientcemois= Math.Round((double)countreseravtion / client * 100);
            ViewBag.pourcentageclientcemois = pourcentageclientcemois;
            ///count comment pos
            var commentpos = (from c in db.commentaire
                              join h in db.hotel on c.id_hotel equals h.id_hotel
                              where c.value == 1 && h.id_personne == id
                              select c).Count();
            ViewBag.commentpos = commentpos;

            ///count comments negativ
            var commentnega = (from c in db.commentaire
                               join h in db.hotel on c.id_hotel equals h.id_hotel
                               where c.value == 0 && h.id_personne == id
                               select c).Count();
            ViewBag.commentnega = commentnega;
            //pourcentage
            var somme = commentnega + commentpos;
            ViewBag.somme =somme;
            var pourcentagepos = Math.Round((double)commentpos / somme * 100);
            ViewBag.pourcentagepos = pourcentagepos;
            var pourcentagenega = Math.Round((double)commentnega / somme * 100);
            ViewBag.pourcentagenega = pourcentagenega;
                }
                else if (typeuser == "client")
                {
                    return View("AccueilCLient", "Client");
                }
            }
            else if (id == 0)
            {
                return RedirectToAction("Authentification");
            }
            return View();
        }
       
        // GET: Personne/Details/5
        public ActionResult Details(int id)
        {
            return View();
        }

        // GET: Personne/Create
        public ActionResult Create()
        {
            return View();
        }

        // POST: Personne/Create
        [HttpPost]
        [ValidateAntiForgeryToken]
        public ActionResult Create([Bind(Include = "id_personne,first_name,last_name,email,password,phone,address")] personne personne)
        {

            // TODO: Add insert logic here
            if (ModelState.IsValid)
            {
                personne.typeuser = "client";
                db.personne.Add(personne);
                db.SaveChanges();
                return RedirectToAction("Authentification");
            }

            return View(personne);

        }

        public ActionResult Authentification()
        {
            var username = Request.Form["email"];
            var password = Request.Form["password"];
            var personne = db.personne
                .SingleOrDefault(u => u.email == username && u.password == password);

            if (personne != null)
            {
                string id = personne.id_personne.ToString();
                HttpContext.Session["id"] = id;
                string typeuser = personne.typeuser;
                HttpContext.Session["typeuser"] = typeuser;
                if (personne.typeuser == "admin")
                {
                    
                    return RedirectToAction("Index");
                }
                else if (personne.typeuser == "client") {

                    return RedirectToAction("AccueilCLient");
                }
            }
            
                return View(personne);
        }
        public ActionResult Rescli()
        {
            string typeuser = (string)HttpContext.Session["typeuser"];
            string id= (string)HttpContext.Session["id"];
            int idr = Convert.ToInt32(id);
            var hotelids = (from r in db.reservation
                            join ro in db.rooms on r.id_room equals ro.id_room
                            join h in db.hotel on ro.id_hotel equals h.id_hotel
                            where r.id_personne == 1
                            select ro.id_room).ToList();
            ViewBag.hotelids = hotelids;

ViewBag.ift = idr;

            var roomsid = (from r in db.reservation
                                   join ro in db.rooms on r.id_room equals ro.id_room
                                   
                                   where r.id_personne == 1
                                   select ro.id_room).ToList();
                    ViewBag.roomsid = roomsid;
                    var roomstype = (from r in db.reservation
                                     join ro in db.rooms on r.id_room equals ro.id_room
                                     
                                     where r.id_personne == 1
                                     select ro.type_room).ToList();
                    ViewBag.roomstype = roomstype;

                    var datedebut = (from r in db.reservation
                                     join ro in db.rooms on r.id_room equals ro.id_room
                                     
                                     where r.id_personne == 1
                                     select r.date).ToList();
                    ViewBag.datedebut = datedebut;
                    var duree = (from r in db.reservation
                                 join ro in db.rooms on r.id_room equals ro.id_room
                                 where r.id_personne == 1
                                 select r.duree).ToList();
                    ViewBag.duree = duree;
                    var nomclient = (from r in db.reservation
                                     join ro in db.rooms on r.id_room equals ro.id_room
                                     join p in db.hotel on ro.id_hotel equals p.id_hotel
                                     where r.id_personne == 1
                                     select p.name_hotel).ToList();
                    ViewBag.nomclient = nomclient;
                    var prenomclient = (from r in db.reservation
                                        join ro in db.rooms on r.id_room equals ro.id_room
                                        join p in db.personne on r.id_personne equals p.id_personne
                                        where r.id_personne == 1
                                        
                                        select p.first_name).ToList();
                    ViewBag.prenomclient = prenomclient;
                    var resid = (from r in db.reservation
                                 join ro in db.rooms on r.id_room equals ro.id_room
                                 where r.id_personne == 1
                                 select r.id_reservation).ToList();
                    ViewBag.resid = resid;
                    
                
               
          
            return View();
        }
        public ActionResult AccueilClient()
        {
            int idddd = 1;
            ViewBag.idddd = idddd;
            string typeuser = (string)HttpContext.Session["typeuser"];
            string idd = (string)HttpContext.Session["id"];
            int id = Convert.ToInt32(idd);
            ViewBag.dio = id;
            
            if (id != 0) {
                var id_hotel = (from ph in db.photo
                                join h in db.hotel on ph.id_hotel equals h.id_hotel
                                where ph.type_photo == "principal" && h.ville=="casablanca" 
                                select h.id_hotel).ToList();

                var images = (from ph in db.photo
                              join h in db.hotel on ph.id_hotel equals h.id_hotel
                              where ph.type_photo == "principal" && h.ville == "casablanca"
                              select ph.nom_photo).ToList();
                var nom_hotel = (from ph in db.photo
                                 join h in db.hotel on ph.id_hotel equals h.id_hotel
                                 where ph.type_photo == "principal" && h.ville == "casablanca"
                                 select h.name_hotel).ToList();
                ViewBag.id_hotel = id_hotel;
                ViewBag.images = images;
                ViewBag.nom_hotel = nom_hotel;

                var id_hotelour = (from ph in db.photo
                                join h in db.hotel on ph.id_hotel equals h.id_hotel
                                where ph.type_photo == "principal" && h.ville == "ourzezat"
                                   select h.id_hotel).ToList();

                var imagesour = (from ph in db.photo
                              join h in db.hotel on ph.id_hotel equals h.id_hotel
                              where ph.type_photo == "principal" && h.ville == "ourzezat"
                                 select ph.nom_photo).ToList();
                var nom_hotelour = (from ph in db.photo
                                 join h in db.hotel on ph.id_hotel equals h.id_hotel
                                 where ph.type_photo == "principal" && h.ville == "ourzezat"
                                    select h.name_hotel).ToList();
                ViewBag.id_hotelour = id_hotelour;
                ViewBag.imagesour = imagesour;
                ViewBag.nom_hotelour = nom_hotelour;
                var id_hotell = (from ph in db.photo
                              join h in db.hotel on ph.id_hotel equals h.id_hotel
                              join ro in db.rooms on h.id_hotel equals ro.id_hotel
                              join r in db.reservation on ro.id_room equals r.id_room
                              join p in db.personne on r.id_personne equals p.id_personne
                              where r.id_personne == id && ph.type_photo == "principal"
                              select h.id_hotel).ToList();

                  var photoo = (from ph in db.photo
                               join h in db.hotel on ph.id_hotel equals h.id_hotel
                               join ro in db.rooms on h.id_hotel equals ro.id_hotel
                               join r in db.reservation on ro.id_room equals r.id_room
                               join p in db.personne on r.id_personne equals p.id_personne
                               where r.id_personne == id && ph.type_photo =="principal"
                               select ph.nom_photo).ToList();


                  var name_hotell = (from ph in db.photo
                                    join h in db.hotel on ph.id_hotel equals h.id_hotel
                                    join ro in db.rooms on h.id_hotel equals ro.id_hotel
                                    join r in db.reservation on ro.id_room equals r.id_room
                                    join p in db.personne on r.id_personne equals p.id_personne
                                    where r.id_personne == id && ph.type_photo == "principal"
                                    select h.name_hotel).ToList();



                  // var model = db.hotel.Find(id);



                  ViewBag.id_hotell = id_hotell;
                  ViewBag.name_hotell = name_hotell;
                  ViewBag.photoo = photoo;

            }
            else if (id == 0)
            {
                var id_hotel = (from ph in db.photo
                                join h in db.hotel on ph.id_hotel equals h.id_hotel
                                where ph.type_photo == "principal" && h.ville == "casablanca"
                                select h.id_hotel).ToList();

                var images = (from ph in db.photo
                              join h in db.hotel on ph.id_hotel equals h.id_hotel
                              where ph.type_photo == "principal" && h.ville == "casablanca"
                              select ph.nom_photo).ToList();
                var nom_hotel = (from ph in db.photo
                                 join h in db.hotel on ph.id_hotel equals h.id_hotel
                                 where ph.type_photo == "principal" && h.ville == "casablanca"
                                 select h.name_hotel).ToList();
                ViewBag.id_hotel = id_hotel;
                ViewBag.images = images;
                ViewBag.nom_hotel = nom_hotel;
                var id_hotelour = (from ph in db.photo
                                   join h in db.hotel on ph.id_hotel equals h.id_hotel
                                   where ph.type_photo == "principal" && h.ville == "ourzezat"
                                   select h.id_hotel).ToList();

                var imagesour = (from ph in db.photo
                                 join h in db.hotel on ph.id_hotel equals h.id_hotel
                                 where ph.type_photo == "principal" && h.ville == "ourzezat"
                                 select ph.nom_photo).ToList();
                var nom_hotelour = (from ph in db.photo
                                    join h in db.hotel on ph.id_hotel equals h.id_hotel
                                    where ph.type_photo == "principal" && h.ville == "ourzezat"
                                    select h.name_hotel).ToList();
                ViewBag.id_hotelour = id_hotelour;
                ViewBag.imagesour = imagesour;
                ViewBag.nom_hotelour = nom_hotelour;
            }




            return View();
        }
        public ActionResult deconnexion()
        {
            Session.Abandon();
            return View("Authentification");
        }

        // GET: Personne/Edit/5
        public ActionResult Edit(int id)
        {
            return View();
        }

        // POST: Personne/Edit/5
        [HttpPost]
        public ActionResult Edit(int id, FormCollection collection)
        {
            try
            {
                // TODO: Add update logic here

                return RedirectToAction("Index");
            }
            catch
            {
                return View();
            }
        }

        // GET: Personne/Delete/5
        public ActionResult Delete(int id)
        {
            return View();
        }

        // POST: Personne/Delete/5
        [HttpPost]
        public ActionResult Delete(int id, FormCollection collection)
        {
            try
            {
                // TODO: Add delete logic here

                return RedirectToAction("Index");
            }
            catch
            {
                return View();
            }
        }


        


    }
}
