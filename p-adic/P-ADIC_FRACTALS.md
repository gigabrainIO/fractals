P-Adisen fraktaalien analyysi on toteutettu Matti Pitkäsen sekä OpenAI ChatGPT kanssa toteuttamalla ohjelman joka visualisoi fraktaalit. Aineiston on koonnut Tuomas Sorakivi.

[TGD P-Adic analysis](TGD%20P-Adic%20analysis.ipynb)

Analysoidaan ohjelma puhtaasti matemaattisena konstruktiona. Ohjelma ei itse asiassa laske "aitoaa" p-adista funktiota, vaan **reaalivälistä [0,1) indusoitua diskreettiä funktiota**, joka syntyy kanonisen numerojärjestys-identifikaation kautta.

Kirjoitetaan koko prosessi matemaattisena kompositiona.

---

# 1. Reaaliluku → p-kantainen kehitelmä

Funktio

```
real_to_base_p(x, p, N)
```

ottaa ( x \in [0,1) ) ja muodostaa sen p-kantaisen murtolukuesityksen:

$$
x = \sum_{k=1}^{\infty} a_{-k} p^{-k},
\qquad a_{-k} \in {0,\dots,p-1}.
$$

Ohjelma katkaisee tämän tasolle (N):

$$
x_N = \sum_{k=1}^{N} a_{-k} p^{-k}.
$$

Tämä on vain tavallinen p-kantainen katkaistu kehitelmä.

---

# 2. Kanoninen identifikaatio

Funktio

```
canonical_I(coeffs, p)
```

laskee

$$
y = \sum_{k=-1}^{-N} a_k p^{-k}.
$$

Koska (k=-m), saadaan

$$
y = \sum_{m=1}^{N} a_{-m} p^{m}.
$$

Tämä on **numero, jonka p-kantaiset numerot ovat täsmälleen x:n murtoluvun numerot — mutta käännettynä kokonaislukupuolelle.**

Siis:

$$
x = 0.a_{-1} a_{-2} a_{-3} \dots \quad (p\text{-kannassa})
$$

↓

$$
y = a_{-1}p + a_{-2}p^2 + \dots + a_{-N}p^N.
$$

Huomio:

$$
0 \le y < p^{N+1}.
$$

Tämä ei ole p-adinen raja-arvo vaan tavallinen kokonaisluku.

Täten määritellään diskreetti identifikaatio:

$$
I_N : [0,1) \to {0,\dots,p^{N+1}-1}.
$$

---

# 3. "p-adinen funktio"

```
padic_f(y1,y2) = y1^2 + y2^2
```

Tämä on täysin tavallinen polynomi ( \mathbb{Z} )-renkaassa:

$$
f(y_1,y_2) = y_1^2 + y_2^2.
$$

Mitään p-adista metriikkaa ei käytetä — vain aritmetiikkaa modulo (p^N).

---

# 4. Takaisin reaalilukuun

Funktio

```
I_to_real(n,p,N)
```

ottaa kokonaisluvun (n) ja muodostaa sen p-kantaiset alimmat (N) numerot:

$$
n = \sum_{k=0}^{\infty} b_k p^k,
$$

ja palauttaa

$$
r = \sum_{k=0}^{N-1} b_k p^{-k}.
$$

Tämä on jälleen numerojärjestys-peilaus:

kokonaisluvun alimmat p-numerot → reaalinen murtoluku.

---

# 5. Koko kompositio

Koko algoritmi laskee funktion

$$
F_N(x_1,x_2) =
I_N^{-1}
\Big(
f\big(
I_N(x_1), I_N(x_2)
\big)
\Big),
$$

missä

$$
I_N(x) =
\sum_{m=1}^{N} a_{-m}(x) p^{m}.
$$

Siis:

1. Lue x:n murtoluvun p-numerot
2. Tee niistä kokonaisluku
3. Laske (y_1^2 + y_2^2)
4. Lue tuloksen alimmat p-numerot
5. Muunna ne takaisin murtoluvuksi

---

# 6. Mitä funktio oikeasti tekee?

Kirjoitetaan eksplisiittisesti:

Olkoon

$$
x_i = \sum_{m=1}^{N} a_{i,m} p^{-m}.
$$

Tällöin

$$
y_i = \sum_{m=1}^{N} a_{i,m} p^m.
$$

Sitten

$$
f = y_1^2 + y_2^2.
$$

Mutta takaisin reaaliksi palautetaan

$$
F_N(x_1,x_2) =
\sum_{k=0}^{N-1}
\big(
(y_1^2+y_2^2)_k
\big)
p^{-k},
$$

missä ((\cdot)_k) on p-kantainen numero kohdassa (k).

Toisin sanoen:

$$
F_N(x_1,x_2) =
\frac{(y_1^2+y_2^2) \bmod p^N}{p^0}
\quad \text{luettuna murtolukuna}.
$$

---

# 7. Rakenteellinen tulkinta

Tämä on täsmälleen:

$$
F_N(x_1,x_2) =
\phi_N\Big(
(I_N(x_1)^2 + I_N(x_2)^2) \bmod p^N
\Big),
$$

missä (\phi_N) on numeroiden kääntöoperaatio.

Siis kyseessä on

> **polynomikuvaus diskreetillä toruksella**
>
> $$
> (\mathbb{Z}/p^N\mathbb{Z})^2
> \to
> \mathbb{Z}/p^N\mathbb{Z}.
> ]$$

Ja grafiikka syntyy, kun tämä vedetään takaisin yksikköneliöön.

---

# 8. Miksi kuva näyttää fraktaalilta?

Koska:

* Polynomikuvaus modulo (p^N) aiheuttaa

  * voimakasta carry-propagointia
  * epälineaarista numerosekoittumista
* Pienet muutokset korkeissa p-adisissa numeroissa vaikuttavat
  matalan tason numeroihin neliöinnissä

Kun tämä heijastetaan takaisin murtolukuihin, saadaan:

* itseään toistavia kuvioita
* p-ary blokkirakennetta
* N-tason resoluutiohierarkia

Erityisesti:

$$
N \to \infty
$$

rajassa syntyy funktio, joka on

* erittäin epäjatkuva reaalimetrissä
* mutta jatkuva p-adisessa metriikassa

---

# 9. Onko tämä aidosti p-adinen funktio?

Ei täysin.

Ohjelma käyttää:

* p-adista numerojärjestystä
* mutta reaalista kokonaislukuaritmetiikkaa
* eikä p-adista normia tai täydellisyyttä

Se simuloi

$$
\mathbb{Z}_p
$$

rakennetta äärellisellä tarkkuudella:

$$
\mathbb{Z}/p^N\mathbb{Z}.
$$

---

# 10. Syvempi tulkinta

Kokonaisuutena ohjelma konstruoi:


$$
F_N =
I_N^{-1}
\circ
f
\circ
I_N,
$$

eli konjugoi tavallisen polynomifunktion p-kantaisen numerojärjestyksen kautta.

Tämä on numerojärjestyksen itseisomorfismi, joka rikkoo reaalitopologian.

---

# 11. Tiivistetty johtopäätös

Ohjelma laskee funktion

$$
F_N(x_1,x_2) =
\text{digit-reversal}
\Big(
\big(
\text{digit-reversal}(x_1)^2
+
\text{digit-reversal}(x_2)^2
\big)
\bmod p^N
\Big).
$$

Se on:

* diskreetti p-adinen polynomikuvaus
* modulo (p^N)
* reaalikoordinaatteihin projisoituna

ja tästä syntyy havaittu fraktaalinen rakenne.
