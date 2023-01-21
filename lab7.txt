#lab7 zadanie 1.2
show create table wikingowie.uczestnicy;
#podzapytanie lub left join
select k.nazwa, k.idKreatury, u.id_uczestnika 
from wikingowie.kreatura k
left join  wikingowie.uczestnicy u ON k.idKreatury=u.id_uczestnika
where u.id_uczestnika is null;

#lab7 zadanie 1.3
SELECT w.nazwa, sum(e.ilosc) FROM wikingowie.wyprawa w
inner join wikingowie.uczestnicy u ON w.id_wyprawy=u.id_wyprawy
inner join wikingowie.ekwipunek e ON u.id_uczestnika=e.idKreatury
group by w.id_wyprawy;

#lab7 zadanie 2.1
select w.nazwa, count(u.id_uczestnika), 
group_concat(k.nazwa separator ' | ')
from wikingowie.wyprawa w 
join wikingowie.uczestnicy u ON u.id_wyprawy=w.id_wyprawy
join wikingowie.kreatura k ON k.idKreatury=u.id_uczestnika
group by w.id_wyprawy;

#lab7 zadanie 2.2
select w.nazwa, w.data_rozpoczecia, k.nazwa as kierownik, ew.kolejnosc, s.nazwa
from wikingowie.etapy_wyprawy ew 
join wikingowie.sektor s ON ew.sektor=s.id_sektora
join wikingowie.wyprawa w ON w.id_wyprawy=ew.idWyprawy
join wikingowie.kreatura k ON k.idKreatury=w.kierownik
order by w.data_rozpoczecia desc, ew.kolejnosc asc;


#lab7 zadanie 3.1
select count(id_sektora) from wikingowie.sektor;
select count(distinct sektor) from wikingowie.etapy_wyprawy;

select nazwa, ifnull(waga,'bez wagi') from wikingowie.kreatura;
select nazwa, if(waga is null,'bez wagi',waga) 
from wikingowie.kreatura;

select s.nazwa, ifnull(count(ew.sektor),'0') as ilosc_odwiedzin
FROM wikingowie.sektor s
left join wikingowie.etapy_wyprawy ew ON ew.sektor=s.id_sektora 
GROUP BY s.id_sektora;

#lab7 zadanie 3.2
SELECT k.nazwa, 
if(count(u.id_wyprawy)=0,
'nie brał udziału w wyprawie','Brał udział w wyprawie') 
FROM wikingowie.kreatura k
LEFT JOIN wikingowie.uczestnicy u  ON k.idkreatury=u.id_uczestnika
group by k.nazwa;


#lab7 zadanie 4.1
SELECT w.nazwa, sum(length(ew.dziennik)) 
FROM wyprawa w inner join etapy_wyprawy ew 
on w.id_wyprawy=ew.idWyprawy GROUP BY w.nazwa 
having sum(length(ew.dziennik)) <400;
#lab7 zadanie 4.2
SELECT w.nazwa, sum(e.ilosc*z.waga) / count(distinct u.id_uczestnika) 
from uczestnicy u left join wyprawa w on w.id_wyprawy=u.id_wyprawy left 
join kreatura k on k.idKreatury=u.id_uczestnika left join ekwipunek e 
on e.idKreatury=k.idKreatury left join zasob z on z.idZasobu=e.idZasobu 
group by w.id_wyprawy;

#lab7 zadanie 5.1
select k.nazwa, w.nazwa, datediff(data_rozpoczecia, dataUr)
from kreatura k join uczestnicy u
ON u.id_uczestnika=k.idKreatury join wyprawa w
ON w.id_wyprawy=u.id_wyprawy join etapy_wyprawy ew 
ON ew.idWyprawy=w.id_wyprawy join sektor s 
ON s.id_sektora=ew.sektor WHERE s.nazwa="Chatka dziadka";
