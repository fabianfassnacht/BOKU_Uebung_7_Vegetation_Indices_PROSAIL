## Strahlungstransfermodellierung mit PROSAIL

### Überblick

In diesem Tutorial lernen wir das PROSAIL-Strahlungstransfermodell für
Vegetationsoberflächen kennen. Dieses Modell ist in R verfügbar, und wir
werden lernen, wie man damit Vorwärtssimulationen durchführt.
Strahlungstransfermodelle wie PROSAIL fassen den aktuellen Wissensstand
über die Wechselwirkung zwischen elektromagnetischer Strahlung im
Wellenlängenbereich von 400 bis 2500 nm und Vegetation zusammen. Sie
sind ein interessantes Werkzeug für wissenschaftliche Studien, aber
auch, um besser zu verstehen, wie bestimmte Pflanzeneigenschaften die
Reflexionseigenschaften von Vegetation beeinflussen.

Im heutigen theoretischen Teil haben Sie bereits etwas mehr über PROSAIL
gelernt. Falls Sie an einer noch detaillierteren Erklärung seiner
Funktionalitäten interessiert sind, habe ich ein umfassendes
Review-Paper zu PROSAIL (Jacquemoud et al. 2009) auf ILIAS hochgeladen.
Dieses Review wurde von den Hauptentwicklern des Modells verfasst.

### Lernziele

-   Ausführen von PROSAIL im Vorwärtsmodus\
-   Verstehen, wie die in PROSAIL implementierten Pflanzeneigenschaften
    die Reflexionseigenschaften von Vegetation beeinflussen\
-   Ein besseres Verständnis des NDVI

### Verwendete Datensätze

In diesem Tutorial werden keine externen Datensätze verwendet.

### Schritt 1: Ausführen von PROSAIL im Vorwärtsmodus

``` r
install.packages("hsdar")
library(hsdar)
```

``` r
?PROSAIL
```

``` r
veg_spectrum = spectra(PROSAIL(LAI=7, Cab = 40, Car=10, Cw = 0.1, Cm = 0.005, N=2, TypeLidf = 2, lidfa = 55))
```

``` r
plot(400:2500, veg_spectrum[1,], type="l", ylab="reflectance", xlab="wavelength [nm]", ylim=c(0,0.5), xlim=c(400,2500))
grid()
```

### Schritt 2: Einfluss einzelner Parameter untersuchen

``` r
parameter <- data.frame(LAI = seq(1,5, length.out=10))
```

``` r
veg_spectra = spectra(PROSAIL(parameterList = parameter))
```

``` r
plot(400:2500, ylab="reflectance", xlab="wavelength [nm]", ylim=c(0,0.5), xlim=c(400,2500))
for(i in 1:nrow(veg_spectra)){
  lines(400:2500, veg_spectra[i,])
}
grid()
```

### Schritt 3: Simulation von NDVI-Werten

``` r
veg_spectrum_1 = spectra(PROSAIL(LAI=5, Cab = 25, Car=10, Cw = 0.05, Cm = 0.005, N=2, TypeLidf = 2, lidfa = 55))[1,]
```

``` r
nir_band = 900-399
red_band = 650-399
```

``` r
plot(400:2500, veg_spectrum_1, type="l", ylab="reflectance", xlab="wavelength [nm]", ylim=c(0,0.5), xlim=c(400,2500), lwd=2)
grid()
abline(v=nir_band+399, lty=2, col="darkred", lwd=2)
abline(v=red_band+399, lty=2, col="red", lwd=2)
```

``` r
ndvi_1 = (veg_spectrum_1[nir_band] - veg_spectrum_1[red_band]) / (veg_spectrum_1[nir_band] + veg_spectrum_1[red_band])
round(ndvi_1,2)
```

### Aufgabe

Versuchen Sie, NDVI-Werte von 0.20, 0.40 und 0.80 zu erzeugen und
analysieren Sie die Parameterkombinationen.

### Abschluss

Dieses Tutorial zeigt die Grundlagen der Nutzung von PROSAIL zur
Simulation von Vegetationsspektren und NDVI.
