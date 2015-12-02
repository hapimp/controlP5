#Processing og ControlP5 workshop

Formål: Formålet med denne workshop er at få Control P5-biblioteket til at fungere med Processing 2.2.1 og 3.x, samt lære, hvordan man kan bruge biblioteket i sine projekter.

###Hvad er ControlP5?
ControlP5 er et Processing-bibliotek, der gør det nemt at bygge GUI-elementer ind i sine sketches. Der er adgang til en lang række tekstfelter, knapper, sliders, lister o.m.a.

Det er muligt at kode den slags ting selv, selvfølgelig, men ControlP5 er en genvej, der gør at du hurtigere kan få bygget den del af dit projekt.

###Inden du starter
Det eneste du behøver for at bruge ControlP5 er Processing og ControlP5-biblioteket:
- [Processing](https://processing.org)
- [ControlP5](http://www.sojamo.de/libraries/controlP5)

På ControlP5's [Github side](https://github.com/sojamo/controlp5) er der en masse god info at hente, og biblioteket kan også hentes herfra.

I skrivende stund er ControlP5 kun testet med Processing 2.2.1, men det virker typisk også fint i 3.0.x, så prøv med det først, hvis du allerede bruger en af disse versioner.

###Installation
ControlP5 kan enten installeres på gammeldags manér, ved at downloade biblioteket og kopiere det til Processings 'libraries'-mappe (og huske at genstarte Processing efter dette - [læs guide her](https://github.com/processing/processing/wiki/How-to-Install-a-Contributed-Library#manual-install) - eller du kan bruge den indbyggede Package Manager ved at vælge Sketch > Import Library > Add Library... og herefter søge efter ControlP5 og installere med et klik på knappen.

###I gang med ControlP5
ControlP5 har en lang række indbyggede eksempler, som du bør åbne og sikre at de kan afvikles i din version af Processing. Når det lykkes er du klar til  at begynde at bygge dine egne interfaces.

Herunder er et par eksempler på hvad du kan bruge ControlP5 til.

**Styre opacity på et billede med en slider eller en knob**
I dette eksempel kan du styre gennemsigtighed på et billede med en ControlP5 slider eller en ControlP5 knob.

```
/**
 * Dette eksempel demonstrerer hvordan du kan bruge ControlP5-biblioteket
 * til at styre gennemsigtigheden på et billede. Det er en sammenskrivning af
 * Processings 'tint'-eksempel: https://processing.org/reference/tint_.html og
 * Slider og Knob-eksemplerne fra ControlP5:
 * http://www.sojamo.de/libraries/controlP5/#examples
**/

import controlP5.*;
PImage img;
Knob myKnobA;
ControlP5 cp5;

int sliderValue = 255;

void setup() {
  size(700,400);
  noStroke();
  cp5 = new ControlP5(this);
  
  // Tilføj en horisontal slider hvis værdi er linket til
  // 'sliderValue'-variablen 
  cp5.addSlider("sliderValue")
     .setPosition(50,50)
     .setRange(0,255)
     ;
     
  myKnobA = cp5.addKnob("knob")
               .setRange(0,255)
               .setValue(255)
               .setPosition(50,70)
               .setRadius(50)
               .setDragDirection(Knob.HORIZONTAL)
               ;
               
  img = loadImage("ddlab.jpg");  // Indlæs et billede i programmet 
}

void draw() {
  background(255,255,255); // Sæt baggrundsfarven til hvid.
  tint(255, sliderValue);// Vælg hvid som tintfarven (ændr ikke farve) og gennemsigtighed til sliderValue
  //tint(255, myKnobA.getValue());  // Fjern denne kommentar for at aktivere 'knob' i stedet for slider
  image(img, 0, 0);  // Vis billedet
}
```

**Styre hastigheden på en videofil**
Samme model kan bruges til at styre hastigheden på afspilningen af en videofil.

```
/**
 * Dette eksempel demonstrerer hvordan du kan bruge ControlP5-biblioteket
 * til at styre hastigheden på afspilningen af en videofil. Det er en sammenskrivning af
 * Processings 'movie speed'-eksempel: https://processing.org/reference/libraries/video/Movie_speed_.html
 * og Slider-eksemplet fra ControlP5:
 * http://www.sojamo.de/libraries/controlP5/#examples
 *
 * Hvis du ikke tidligere har arbejdet med video i Processing, skal du installere
 * video-biblioteket via Sketch > Import Library > Add Library... og søge efter video.
 *
 * Din video skal ligge i en undermappe med navnet 'data' i den mappe hvor du har din sketch. 
**/

import processing.video.*;
import controlP5.*;

Movie myMovie;
ControlP5 cp5;
int sliderValue = 1;

void setup() {
  size(1024, 576);
  myMovie = new Movie(this, "laser.mp4");
  myMovie.loop();
  
  cp5 = new ControlP5(this);
  
  // Tilføj en horisontal slider hvis værdi er linket til
  // 'sliderValue'-variablen 
  cp5.addSlider("sliderValue")
   .setPosition(50,50)
   .setRange(1,4) //vi sætter sliderens værdi til mellem 1 og  4.
   .setNumberOfTickMarks(4) //vi sætter 4 mærker på slideren så vi kun får de hele tal
   ;
}

void draw() {
  frameRate(30);
  tint(255, 20);
  image(myMovie, 0, 0);
  myMovie.speed(sliderValue); //Vi sætter hastigheden til mellem 1 (100%) og 4 (400%)
}

// Called every time a new frame is available to read
void movieEvent(Movie m) {
  m.read();
}
```

**Ændre mængden af objekter på et canvas**
I dette eksempel ændrer vi mængden af cirkler på et canvas alt efter hvad slideren viser.

```
/**
 * Dette eksempel demonstrerer hvordan du kan bruge ControlP5-biblioteket
 * til at styre mængden af objekter på dit canvas.
 *
 * Sketchen tager udgangspunkt i Slider-eksemplet fra ControlP5:
 * http://www.sojamo.de/libraries/controlP5/#examples
**/

import controlP5.*;

ControlP5 cp5;
int sliderValue = 2;
int oldValue = sliderValue; //Vi laver en variabel, som vi sætter lig med sliderValue

void setup() {
  size(1024, 576);
  noStroke();
  smooth();
  cp5 = new ControlP5(this);
  
  // Tilføj en horisontal slider hvis værdi er linket til
  // 'sliderValue'-variablen 
  cp5.addSlider("sliderValue")
   .setPosition(50,50)
   .setRange(1,40) //vi sætter sliderens værdi til mellem 1 og  4.
   .setNumberOfTickMarks(40) //vi sætter 4 mærker på slideren så vi kun får de hele tal
   ;
   drawCircles(); //Vi kalder drawCircles-funktionen for at få tegnet de første cirkler
}

void draw() {
  //Vi kalder drawCircles hvis oldValue-variablen ikke er lig med drawCircles.
  if (oldValue != sliderValue) { 
    drawCircles();
  }
}

void drawCircles() {
    background(0,0,0); //Vi 'sletter' de gamle cirkler ved at tegne en sort baggrund
    oldValue = sliderValue; //Vi sætter oldValue lig med sliderValue igen
  
  //Vi skaber en ellipse med en random placering og størrelse, så længe 'i' er mindre end sliderValue
  for (int i = 0; i < sliderValue; i = i+1) {
    float circleX = random(900);
    float circleY = random(450);
    float circleSize = random(60);  
    ellipse(circleX, circleY, circleSize, circleSize);
  }

}
```


