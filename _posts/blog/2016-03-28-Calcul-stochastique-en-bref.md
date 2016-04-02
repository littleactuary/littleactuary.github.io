---
layout: post
title: "Calcul stochastique en bref"
modified:
categories: blog
excerpt: "Les fondamanteux du calcul stochastique appliqué à la finance"
tags: [calcul stochastique, finance, stochastic calculus, financial calculus, Ito, Brownian motion, Black Scholes]
image:
  feature:
date: 2016-03-28T08:08:50-04:00
---

Récemment, un ami à moi m'a demandé de lui expliquer rapidement les calculs stochastiques pour préparer son examen en M2 Finance. Je lui ai répondu que ça faisait 2 ans que je ne les ai pas touchés et que je ne retenais que dalle de ces fameux calculs. Finalement, j'ai quand même passé un soir à reparcourir un poly de cours sur les principes de bases en calcul stochastique. Ce qui m'a étonné est que j'ai eu l'impression de les avoir mieux compris qu'il y a deux trois ans quand je les faisais à l'école. 

Bref, je voulais expliquer en quelques mots (je m'en doute) les fondamentaux du calcul stochastique, appliqué à la finance surtout, en me focalisant sur leurs interprétations et en négligeant toutes les démonstrations ainsi que des notions mathématiques abstraites. 

* Table of Contents
{:toc}

## Rappels mathématiques

Pour comprendre le calcul stochastique, nous ne pouvons quand même pas négliger tout le côté mathématique. Nous allons revoir les notions de martingale, 

# Quelques notions de bases

Un **monde risque neutre** correspond à un espace de probabilité fictf où le prix d'un produit vu d'aujourdui peut être calculé comme l'espérance de son prix futur actualisé.

\\[ P_0  = \mathcal{E}[P_t] \times facteur d'actualisation(0,t) \\]