# PimpMyIdf
Design an A.V.E.C ("Air + Vortex = Economiseur Carburant") for my 44 IDF carbs


## Design a Vortex with openScad

![PimpMyIdf](https://github.com/coxifred/PimpMyIdf/blob/master/vortex_idf.png?raw=true)

<a href=https://github.com/coxifred/PimpMyIdf/blob/master/vortex_idf.stl>Stl view here</a>

**Use:**

// "Blades" amount, compression (best 20), height, diameter

buildVortex(5,20,40,20);

```scad
/*
Module pour faire un vortex A.V.E.C
Gorille 2018

*/
epaisseur=0.1;




/**
Permet de tracer une barre (lame horizontale d'une pale)
*/
module barre(z,rotation,colorname,degree,total)
{
    color( c = [255, z/255*2, 0,1.0] )
    {
        rotate([0,0,-rotation]) translate([0,0,z*epaisseur])  cube(size = [epaisseur*degree,20,epaisseur]);    
    }
}

/**
Permet de dessiner une pale
*/
module pale(height,degree)
{
    max=height/epaisseur;
    pas=degree/max;
    for ( i = [0:(height/epaisseur) - 1] )
    {
         h=sin(i)* i / 90;
        if ( sin(i-1)* (i-1) / 90 < h || i ==0)
        {
          barre(i,i*pas*h*h,"Green",degree/2.8, (height/epaisseur) );
        }
    }
}



/**
Tube booléen interne
Param 1: diamètre
*/
module tube(diametre)
{
    difference() {
                        cylinder (h = 40, r=diametre/2, center = true, $fn=100);
                        cylinder (h = 40, r=diametre/2 - 2, center = true, $fn=100);
                        }
}

/**
Construction du vortex
Param 1 : nombre de pales du vortex
Param 2 : écrasement des pales.
Param 3 : hauteur du vortex.
Param 4 : diamètre du vortex.
*/
module buildVortex(nbPales,ecrasement,hauteur,diametre)
{
    echo ("nbPales=",nbPales, " ecrasement=",ecrasement," hauteur:",hauteur," diametre:",diametre );
    //intersection entre tube central et pales
    difference() {
		 buildPales(nbPales,ecrasement);
        cylinder (h = 40, r=7, center = true, $fn=100);
}
      translate([0,0,-8])tube(diametre*2+1);
     //resize(newsize=[diametre,diametre,hauteur])  buildPales(nbPales,ecrasement);
}

/**
Construction des pales
Param 1 : nombre de pales
Param 2 : écrasement
*/
module buildPales(nbPales,ecrasement)
{
    degree=360/nbPales;
    for ( p = [1:nbPales] )
    {
    rotate([0,0,-degree*p]) pale(ecrasement,degree,couleur);
    }
}


// Nombre de pales, écrasement (meilleur 20), hauteur,diametre
buildVortex(5,20,40,20);



```
