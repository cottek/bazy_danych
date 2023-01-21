#Zad 1 
SELECT imie,nazwisko, year(data_urodzenia) FROM pracownik;

#zad 2
SELECT imie,nazwisko, (2023-YEAR(data_urodzenia))
FROM pracownik;

#zad 3
SELECT d.nazwa, COUNT(p.id_pracownika) as 'Liczba pracowników'
FROM dzial d JOIN pracownik p 
ON p.dzial=d.id_dzialu GROUP BY d.nazwa;

#zad 4
SELECT k.nazwa_kategori, COUNT(t.id_towaru) 
FROM kategoria k JOIN towar t ON t.kategoria=k.id_kategori 
GROUP BY k.nazwa_kategori;

#zad 5
SELECT k.nazwa_kategori, group_concat(t.nazwa_towaru) 
FROM kategoria k JOIN towar t ON t.kategoria=k.id_kategori 
GROUP BY k.id_kategori;

#zad 6
SELECT ROUND(AVG(pensja),2) as 'srednie zarobki' 
FROM pracownik;

#zad 7
SELECT ROUND(AVG(pensja),2) as 'srednie zarobki' 
FROM pracownik WHERE (2023-YEAR(data_zatrudnienia))>=5;

#zad 8
SELECT * FROM status_zamowienia;

SELECT t.nazwa_towaru, COUNT(t.nazwa_towaru)
FROM zamowienie z
JOIN pozycja_zamowienia pz 
ON pz.id_pozycji=z.id_zamowienia JOIN towar t 
ON t.id_towaru=pz.towar
GROUP BY t.id_towaru  
ORDER BY COUNT(t.nazwa_towaru) DESC LIMIT 10;

Select t.nazwa_towaru, count(pz.towar) 
as ilosc from pozycja_zamowienia pz 
join towar t on pz.towar=t.id_towaru
group by towar order by ilosc desc limit 10;

#zad 9
SELECT SUM(pz.ilosc*pz.cena) FROM zamowienie z
join pozycja_zamowienia pz 
ON z.id_zamowienia=pz.zamowienie
WHERE (z.data_zamowienia)=2017 
AND quarter(z.data_zamowienia)=1
GROUP BY zamowienie;

select z.numer_zamowienia, sum(pz.ilosc*pz.cena) as wartosc from pozycja_zamowienia pz 
join zamowienie z on pz.zamowienie=z.id_zamowienia 
where year(z.data_zamowienia)=2017 and month(z.data_zamowienia) between 1 and 3
group by zamowienie;

#zad 10
SELECT p.imie, p.nazwisko, SUM(pz.ilosc*pz.cena)
 FROM pracownik p 
JOIN zamowienie z
ON z.pracownik_id_pracownika=p.id_pracownika
JOIN pozycja_zamowienia pz 
ON pz.zamowienie=z.id_zamowienia
GROUP BY p.id_pracownika 
ORDER BY SUM(pz.ilosc*pz.cena) DESC;

#--------------------
#zad1
SELECT d.nazwa, MIN(p.pensja), MAX(p.pensja), avg(p.pensja)
FROM dzial d JOIN pracownik p 
ON p.dzial=d.id_dzialu GROUP BY d.id_dzialu;

#zad 2
SELECT k.pelna_nazwa, SUM(pz.ilosc*pz.cena) 
FROM klient k JOIN zamowienie z ON z.klient=k.id_klienta
JOIN pozycja_zamowienia pz 
ON pz.zamowienie=z.id_zamowienia GROUP BY k.id_klienta
LIMIT 10;

#zad 3
SELECT year(z.data_zamowienia) as 'rok', 
round(sum(pz.ilosc*pz.cena),2) as 'wartosc' 
FROM zamowienie z JOIN status_zamowienia sz 
ON sz.id_statusu_zamowienia=z.status_zamowienia
JOIN pozycja_zamowienia pz 
ON pz.zamowienie=z.id_zamowienia 
WHERE sz.nazwa_statusu_zamowienia="zrealizowane"
GROUP BY rok ORDER BY wartosc DESC;

#zad 4
SELECT round(sum(pz.ilosc*pz.cena),2) as 'wartosc' 
FROM zamowienie z JOIN status_zamowienia sz 
ON sz.id_statusu_zamowienia=z.status_zamowienia
JOIN pozycja_zamowienia pz 
ON pz.zamowienie=z.id_zamowienia 
WHERE sz.nazwa_statusu_zamowienia="anulowane";

#zad 7
SELECT year(z.data_zamowienia) as 'rok', 
#round(sum(pz.ilosc*pz.cena)-(t.cena_zakupu*sum(pz.ilosc)),2)
(sum(pz.ilosc*pz.cena)-(pz.ilosc*t.cena_zakupu)) as 'dochod' 
FROM zamowienie z JOIN status_zamowienia sz 
ON sz.id_statusu_zamowienia=z.status_zamowienia
JOIN pozycja_zamowienia pz 
ON pz.zamowienie=z.id_zamowienia JOIN towar t
ON t.id_towaru=pz.towar
WHERE sz.nazwa_statusu_zamowienia="zrealizowane"
GROUP BY rok ORDER BY dochod DESC;

Select year(z.data_zamowienia) as rok, 
(sum(pz.ilosc*pz.cena)-(pz.ilosc*t.cena_zakupu)) as dochod 
from zamowienie z 
join pozycja_zamowienia pz on z.id_zamowienia=pz.zamowienie
join towar t on pz.towar=t.id_towaru
where z.status_zamowienia=5
group by rok;

#zad 9
Select monthname(data_urodzenia) as miesiac, 
count(id_pracownika) as ilosc from pracownik 
group by month(data_urodzenia)
 order by month(data_urodzenia) asc;
 
#zad 10
SELECT imie, nazwisko,
timestampdiff(MONTH,data_zatrudnienia,now())*pensja
as 'koszt' FROM pracownik;
