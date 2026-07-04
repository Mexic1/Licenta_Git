# Infrastructură de Rețea Redundantă și Securizată pentru un Complex Spitalicesc Multi-Site (GNS3)

Acest repository conține aplicația practică a lucrării de licență: proiectarea și implementarea unei infrastructuri de rețea redundante și securizate pentru un complex spitalicesc distribuit pe mai multe locații, simulată integral în GNS3.

**Autor:** Morar Cosmin-Valentin
**Facultate:** Automatică și Calculatoare, Universitatea Politehnica Timișoara
**An / Program:** Anul IV – Ingineria Sistemelor
**Sesiune:** iunie–iulie 2026

---

## Descriere

Infrastructura urmează un model **hub-and-spoke**, în care o clădire centrală (Main Building) reprezintă nucleul rețelei și interconectează site-urile periferice — Emergency Building, Medical Lab, Clinic și Remote Site — la care se adaugă un segment dedicat de DataCenter.

Main Building constituie aria de backbone OSPF (aria 0) și găzduiește serverele de infrastructură: Zabbix (monitorizare), DNS/DHCP, AAA/TACACS+ (autentificare centralizată) și Ansible (automatizare). Fiecare site periferic rulează o arie OSPF proprie și este conectat la clădirea centrală prin tuneluri GRE over IPsec. Segmentul DataCenter este atașat prin GNS3 Cloud și oferă acces la internet, precum și serviciile centralizate de NTP și Syslog.


## Funcționalități implementate

- Segmentare prin VLAN-uri și trunking 802.1Q
- Agregarea legăturilor prin EtherChannel/LACP
- Protecție Layer 2: Rapid PVST+, PortFast, BPDU Guard
- Securizarea porturilor: Port Security (sticky MAC), DHCP Snooping
- Rutare dinamică OSPF pe mai multe arii
- Rutare între ISP-uri prin EIGRP și BGP
- Redundanță de gateway prin HSRP v2
- Tuneluri GRE over IPsec (AES-256, SHA-256, DH group 14)
- Acces la internet prin NAT/PAT
- Filtrarea traficului prin ACL
- Firewall-uri stateful Cisco ASAv
- Acces SSH securizat (RSA 2048 biți)
- Autentificare centralizată prin AAA / TACACS+
- Servicii DHCP (cu relay agents) și DNS (BIND9)
- NTP ierarhic și Syslog centralizat
- Monitorizare prin Zabbix și SNMP
- Automatizare prin Ansible

## Cerințe

**Software:** GNS3, GNS3 VM, VMware Workstation sau VirtualBox, imagini Cisco IOL (routere/switch-uri), imaginea Cisco ASAv 9.16.4.19, imagini Ubuntu Server 20.04/22.04.

**Hardware:** procesor cu suport de virtualizare (Intel VT-x / AMD-V), memorie RAM suficientă pentru rularea simultană a echipamentelor, spațiu pe disc pentru proiecte și imagini.

> **Notă:** Imaginile Cisco IOS și Cisco ASAv **nu** sunt incluse în acest repository, fiind fișiere proprietare. Ele trebuie obținute separat din surse licențiate sau din mediul de laborator și importate manual în GNS3.

## Instalare și rulare

1. Se instalează GNS3 (și GNS3 VM, dacă rularea e locală) sau se folosește un server GNS3 dedicat.
2. Se importă în GNS3 imaginile Cisco IOL, imaginea Cisco ASAv și imaginile Ubuntu.
3. Se importă cele două topologii (`Spital.gns3project` și `DataCenter.gns3project`).
4. Se verifică asocierea corectă a imaginilor și a interfețelor Cloud/GNS3.
5. Se pornesc echipamentele — se recomandă întâi DataCenter-ul, apoi rețeaua spitalului.
6. Se așteaptă câteva minute pentru încărcarea configurațiilor, stabilirea vecinătăților OSPF, ridicarea tunelurilor și pornirea serviciilor.

## Comenzi de verificare

```
show ip interface brief          # starea interfețelor
show ip route                    # tabela de rutare
show ip ospf neighbor            # vecinătăți OSPF
show crypto isakmp sa            # faza 1 IPsec
show crypto ipsec sa             # faza 2 IPsec
show standby brief               # redundanță HSRP
show tacacs                      # starea serverului AAA
show ntp status                  # sincronizare NTP
```

## Backup-urile configurațiilor

Backup-urile din directoarele `configs/` sunt incluse pentru siguranță și documentare. Ele se folosesc doar când un echipament pierde configurația, când o configurație este modificată greșit sau când se dorește refacerea unei topologii. În mod normal, topologiile funcționează direct după import, fără modificări.
