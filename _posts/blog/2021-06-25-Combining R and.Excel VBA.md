---
layout: post
title: "Using Excel to call R script"
modified:
categories: blog
excerpt: "Illustrating with a Principal Component Analysis"
tags: [R, Excel, VBA]
image:

comments: true
share: true
date: 2021-06-25T15:39:55-04:00
---

With many programming languages coexisting in modern day IT, each with their own pros and cons, more and more often we have to combine them in our projects. I once wrote a post about calling C++ from R. Today, I'd like to share a quick way to call R program from Excel (with a few lines of VBA code), what I believe to be useful in many cases.

* Table of Contents
{:toc}

## Background
To way of background, a few years ago, I was asked to modelling the competitive rate ("taux concurrentiel", ou "taux servi moyen" in French) for life saving insurance products, given a historic of interest rates and their moving averages. The first step was to study the correlation of these rates and to figure out which of them are really useful for explaining the competitive rate. Principal Component Analysis (PCA) technique was chosen for the task, and I didn't want to implement the PCA from scratch with Excel and VBA. So, I turned to the FactoMineR package in R for help (of course). The rest of the project needed to be done in Excel.

## Main steps
Here is the idea and the main steps:
0) Some preliminary works need to be done in the main Excel file for input preparation.
1) An input csv file is then built and saved (with VBA code) in the input folder.
2) Then I use VBA to call R portable (just a lightweight version of R, you can use R instead) and run my R script which is in charge of the PCA analysis. for this step, you need to specify (in the Excel file) where to find R portable and your R script. We also supposed that the FactoMineR package has been installed.
3) The output (some graphs built with R) will be saved in the output folder at the end of the R script run.
4) Finally, the VBA code will copy all these graphs and paste them back into the main Excel file.


<center>
<a href='https://postimages.org/' target='_blank'><img src='https://i.postimg.cc/hP18zBj5/r-excel.jpg' border='0' alt='r-excel'/></a>
</center>

## R script

{% highlight %}
rm(list=ls())

ARGS = commandArgs(TRUE)    
Chemin_inputs_csv = toString(ARGS[1])
Chemin_outputs_csv = toString(ARGS[2])

#input folder
setwd(Chemin_inputs_csv)

data<-read.table(file="input.csv", sep=";", header=TRUE, row.names=1, check.names=FALSE, dec = ",")

# install.packages("FactoMineR")
library(FactoMineR)

# PCA
pca<-PCA(data)

# output folder
setwd(Chemin_outputs_csv)

do.call(file.remove, list(list.files(Chemin_outputs_csv, full.names = TRUE)))

#plots
jpeg('graphe_eboulis_vp.jpg')
plot(1:length(pca$eig$eigenvalue), pca$eig$eigenvalue, type="b", ylab="Valeur propre", xlab="Composante p", main="Scree plot")
dev.off()

jpeg('graphe_variance_cumulee.jpg')
plot(1:length(pca$eig$"cumulative percentage of variance"), pca$eig$"cumulative percentage of variance", type="b", ylab="% de la variance expliqué", xlab="Composante p", main="Graphique des % cumulés")
dev.off()

jpeg('graphe_acp_ind.jpg')
plot.PCA(pca, axes=c(1, 2), choix="ind", habillage="ind")
dev.off()

jpeg('graphe_acp_var.jpg')
plot.PCA(pca, axes=c(1, 2), choix="var", habillage="none")
dev.off()

dimen= dimdesc(pca, axes=c(1,2))
write.csv2(dimen$Dim.1$quanti, "axe1.csv")
write.csv2(dimen$Dim.2$quanti, "axe2.csv")

write.infile(pca, file="pca_result.txt", sep="\t")
{% endhighlight %}


## VBA
{% highlight vba %}
Declare Function OpenProcess Lib "kernel32" (ByVal Access As Long, ByVal Handle As Long, ByVal Process As Long) As Long
Declare Function GetExitCodeProcess Lib "kernel32" (ByVal Process As Long, ExitCode As Long) As Long


Public Const AccessType As Long = &H400
Public Const StillActive As Long = &H103

Sub test()

    Dim oshell As Object
    Set oshell = VBA.CreateObject("WScript.Shell")
    Set oEnv = oshell.Environment("PROCESS")

    Dim waitTillComplete As Boolean: waitTillComplete = True
    Dim style As Integer: style = 1
    Dim errorCode As Integer
    Dim path As String

    adresseR = VBA.Chr(34) & Replace(Cells.Range("RhomeDir"), "/", "\") & VBA.Chr(34)
    adresseProg = VBA.Chr(34) & Replace(Cells.Range("MyRscript"), "/", "\") & VBA.Chr(34)
    adresseInp = VBA.Chr(34) & Replace(Cells.Range("Input_path"), "/", "\") & VBA.Chr(34)
    adresseOuT = VBA.Chr(34) & Replace(Cells.Range("Output_path"), "/", "\") & VBA.Chr(34)


    path = adresseR & " " & adresseProg & " " & adresseInp & " " & adresseOuT

    ' désactiver l'avertissement de sécurité
    oEnv("SEE_MASK_NOZONECHECKS") = 1
    ' lancer le script R avec shell
    errorCode = oshell.Run(path, style, waitTillComplete)
    oEnv.Remove ("SEE_MASK_NOZONECHECKS")


    ' Ne pas exécuter la macro VBA tant que l'exécution du code R n'est pas terminée
    Proc = OpenProcess(AccessType, False, ProcessShell)
    Do
        GetExitCodeProcess Proc, ExitCode
        DoEvents
    Loop While ExitCode = StillActive

    ' message
    If errorCode = 0 Then
        MsgBox "L'ACP a été réalisée avec succès sous R. Vous pouvez retrouver l'ensemble des outputs dans " & adresseOuT
    Else
        MsgBox "Le programme n'a pas pu être tourné suite à l'erreur " & errorCode & "."
    End If

    'supprimer toutes les images présentes dans l'onglet
    For Each pic In ActiveSheet.Pictures
        pic.Delete
    Next pic
    'recopier les images issus du programme R dang l'onglet Excel
    Call load_image("graphe_eboulis_vp.jpg", Range("A12"))
    Call load_image("graphe_variance_cumulee.jpg", Range("G12"))
    Call load_image("graphe_acp_ind.jpg", Range("A42"))
    Call load_image("graphe_acp_var.jpg", Range("G42"))
End Sub

Sub load_image(namePic As String, position As Range)

    position.Select
    Dim sFile As String

    sFile = Range("Output_path") & "\" & namePic

    ActiveSheet.Pictures.Insert(sFile).Select
        Selection.ShapeRange.Height = 324
        Selection.ShapeRange.Width = 396
    With Selection.ShapeRange.Line
        .Visible = msoTrue
        .ForeColor.ObjectThemeColor = msoThemeColorText1
        .ForeColor.TintAndShade = 0
        .ForeColor.Brightness = 0
        .Transparency = 0
    End With
End Sub

{% endhighlight %}

You can take a look at the project skeleton [here](https://drive.google.com/drive/folders/1Ts0shFb2GtTiEd-DYb3CpqEwGKY-6N_P?usp=sharing){:target="_blank"}. (It supposes that the input csv file is already available in the input folder).

Voilà!!!
