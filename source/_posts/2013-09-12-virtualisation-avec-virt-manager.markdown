---
layout: post
title: "Virtualisation avec virt-manager"
date: 2013-09-12 16:19
comments: false
sharing: false
categories: [Debian, Virtualisation, libvirt]
---

virt-manager est une interface graphique permettant d'utiliser **libvirt** pour créer des machines virtuelles. Libvirt est un ensemble d'outils permettant la gestion de machines virtuelles et d'autres fonctionnalités utiles de virtualisation. Elle se compose d'une API, d'un ensemble de bindings en différents langages, d'un démon (libvirtd), d'un logiciel en ligne de commande (virsh) et d'une interface graphique (virt-manager). Cet outil permet donc de gérer des machines virtuelles, quels que soient le ou les hyperviseurs en charge de celles-ci, sans apprendre à utiliser tous les outils propres à chaque solution de virtualisation. <!-- more -->

Libvirt permet donc, outre la gestion des machines virtuelles (création, démarrage, arrêt, etc.) :

 - La gestion des interfaces réseaux physiques et virtuelles
 - La gestion de réseaux virtuels NAT, ponts, vlans, etc.
 - La gestion des disques physiques, réseaux, images disques, LVM, etc.
 - La gestion des machines virtuelles, du réseau et des disques à distance, grâce à virsh ou virt-manager KVM, Xen, OpenVZ et LXC, entres autres, sont gérés par Libvirt.

## Installation

L'installation est très simple :

	# aptitude install virt-manager

À la fin de l'installation, ajouter votre utilisateur au groupe `libvirt` et redémarrer votre ordinateur.

	# adduser <user> libvirt

## Création d'une machine virtuelle

Et puis non, vous êtes grand, vous vous débrouillerez.

Bonne journée !

{% img center /images/virt-manager/virt-manager.png %}