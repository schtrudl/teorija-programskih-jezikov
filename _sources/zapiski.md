```markdown
# Algebrajski učinki

## Težave z monadami

Operacijska semantika še manjka, glavni razlog je:
- obravnava specifičnih operacij (`get`, `set`, `raise`, `try`, `choose`)
- težko se kombinirajo ($T_1 : \mathbf{Set} \to \mathbf{Set}$, $T_2 : \mathbf{Set} \to \mathbf{Set}$, toda $T_1 \circ T_2$ ni monada)

$$\eta^{T_1 \circ T_2}_X : X \to T_1(T_2 X)$$

$$\eta^{T_1 \circ T_2}_X(x) = \eta^{T_1}_{T_2 X}\bigl(\eta^{T_2}_X(x)\bigr)$$

Z množenjem imamo težave. Lažje jih je videti pri $\mu_X : TTX \to TX$: imamo $\mu^{T_1}_{T_1 X} : T_1(T_1 X) \to T_1 X$ in $\mu^{T_2}_{T_2 X} : T_2(T_2 X) \to T_2 X$, iz česar lahko dobimo $T_1(T_1(T_2(T_2 X))) \to T_1(T_2 X)$, potrebujemo pa $T_1(T_2(T_1(T_2 X))) \to T_1(T_2 X)$.

## Signature in algebrajske teorije

**Def.** *Signatura* $\Sigma$ za algebrajsko teorijo je seznam operacij in njihovih členosti (= arity):
$$\Sigma = \mathrm{op}_1 : n_1, \dots, \mathrm{op}_k : n_k$$

**Primer.** $\Sigma_{\text{grupa}} = \{\underbrace{m : 2}_{\text{množenje}},\ \underbrace{i : 1}_{\text{inverz}},\ \underbrace{e : 0}_{\text{enota}}\}$

Iz spremenljivk in operacij iz $\Sigma$ lahko sestavljamo izraze, npr. $m(i(x), m(m(y, e()), z))$.

Formalno lahko to definiramo induktivno. Naj $\Gamma = x_1, \dots, x_n$. Sodbe oblike $\Gamma \vdash_\Sigma t$ uvedemo s pravili:

$$\frac{x \in \Gamma}{\Gamma \vdash_\Sigma x} \qquad \frac{\mathrm{op} : n \in \Sigma \quad (\Gamma \vdash_\Sigma t_i)_{i=1}^n}{\Gamma \vdash_\Sigma \mathrm{op}(t_1, \dots, t_n)}$$

**Def.** *Algebrajska teorija* nad signaturo $\Sigma$ je množica enačb med izrazi.

**Primer.** Teorija za grupe:
$$m(x, m(y, z)) = m(m(x, y), z)$$
$$m(x, e()) = x$$
$$m(e(), x) = x$$
$$m(x, i(x)) = e()$$
$$m(i(x), x) = e()$$

**Primer.** Teorija za polmreže: $\Sigma = \{\vee : 2\}$,
$$x \vee (y \vee z) = (x \vee y) \vee z$$
$$x \vee y = y \vee x$$
$$x \vee x = x$$

**Neprimer.** Obsegi: ker imamo $0 \neq 1$ — algebrajska teorija ne dopušča neenačb.

## Interpretacije in modeli

**Def.** *Interpretacija* signature $\Sigma$ je množica $X$ skupaj s funkcijo $[\![\mathrm{op}_i]\!] : X^{n_i} \to X$ za vsak $\mathrm{op}_i \in \Sigma$.

Če imamo interpretacijo, znamo interpretirati vsak izraz nad $\Sigma$.

**Def.** *Model* algebrajske teorije je interpretacija njene signature, ki zadošča vsem enačbam.

**Primer.** Vsaka grupa $G$ je model teorije za grupe.

**Trditev.** Če sta $(X, [\![\mathrm{op}_i]\!]^X)$ in $(Y, [\![\mathrm{op}_i]\!]^Y)$ modela za dano teorijo, je model tudi
$$\bigl(X \times Y,\ [\![\mathrm{op}_i]\!]^{X \times Y} : ((x_1, y_1), \dots, (x_n, y_n)) \mapsto ([\![\mathrm{op}_i]\!]^X(x_1, \dots, x_n),\ [\![\mathrm{op}_i]\!]^Y(y_1, \dots, y_n))\bigr)$$

**Posledica.** Ne obstaja teorija za obsege.

**Def.** *Homomorfizem* med modeloma $X$ in $Y$ je preslikava $h : X \to Y$, da velja
$$h\bigl([\![\mathrm{op}]\!]^X(x_1, \dots, x_n)\bigr) = [\![\mathrm{op}]\!]^Y(h(x_1), \dots, h(x_n))$$

## Prosti modeli

**Def.** *Absolutno prost model* signature $\Sigma$ nad množico $X$ je množica $FX$, sestavljena iz vseh izrazov, ki jih dobimo iz elementov $X$ ter operacij, skupaj z interpretacijo
$$[\![\mathrm{op}]\!]^{FX}(t_1, \dots, t_n) = \mathrm{op}(t_1, \dots, t_n)$$
za vsak $\mathrm{op} : n \in \Sigma$.

**Primer.** Absolutno prost model za signaturo grup nad $X = \{A, B\}$ vsebuje elemente
$$A,\ B,\ e(),\ i(A),\ i(B),\ m(A, A),\ m(A, B),\ m(e(), A),\ \dots$$
$$[\![i]\!]\bigl(i(m(A, B))\bigr) = i(i(m(A, B)))$$

**Def.** *Prost model* $FX$ dobimo tako, da na absolutno prostem modelu podamo ekvivalenčno relacijo, porojeno z dano algebrajsko teorijo, in nato naredimo kvocient po njej.

> Kvocient absolutno prostega modela.

**Primer.** Prost model za grupe nad $\{A, B\}$ izenači $[m(e(), A)] = [A]$, …

- $F_{\text{polmreže}}\, X \cong \mathcal{P}_{\text{fin}}\, X \setminus \{\emptyset\}$ (končne podmnožice), kjer
$$[A],\ [B],\ [C],\ \dots,\ [A \vee B],\ [A \vee C],\ \dots,\ [(A \vee B) \vee C],\ \dots$$
- $\Sigma = \{\vee : 2,\ \bot : 0\}$, teorija za polmreže $+\ x \vee \bot = \bot \vee x = x$. Tedaj je $FX \cong \mathcal{P}_{\text{fin}}\, X$.
- Če odstranimo komutativnost in idempotentnost za $\vee$, dobimo $FX \cong X^\ast$.

## Povezava z monadami

Vidimo, da je prost model za polmreže podoben delu monade za nedeterminizem. V resnici velja še več: vsak prost model porodi monado
$$(F,\ \eta_X = x \mapsto [x],\ \mathbin{>\!\!>\!\!=}_{X,Y})$$
kjer
$$\eta_X : X \to FX,\quad x \mapsto [x]$$
$$\mathbin{>\!\!>\!\!=}_{X,Y} : FX \to (X \to FY) \to FY$$
$$[x] \mathbin{>\!\!>\!\!=} f = f\,x \qquad \text{(DN: preveri, da je dobro definirano)}$$
$$[\mathrm{op}(t_1, \dots, t_n)] \mathbin{>\!\!>\!\!=} f = [\![\mathrm{op}]\!]^{FY}([t_1] \mathbin{>\!\!>\!\!=} f, \dots)$$

> DN: Preveri, da zadošča lastnostim monade.

Elemente absolutno prostega modela lahko vidimo kot sledi izvajanja.

**Primer.** $\Sigma = \{\vee : 2,\ \bot : 0\}$. Tedaj bi lahko program

```
x ← ret 1  ∨  ret 2
y ← ret 10 ∨ ret 20
ret (x + y)
```

predstavili z drevesom:

```
            ∨
       ╱       ╲
     x=1        x=2
    ╱   ╲      ╱   ╲
 y=10  y=20  y=10  y=20
  (11) (21)  (12)  (22)
```

oz. z izrazom $(11 \vee 21) \vee (12 \vee 22)$.

$$F_{\text{state}}\, X \cong S \to S \times X$$
```

Nekaj nejasnosti pri prepisu:
- Na koncu strani 1 sem zapis "Aldar" pod podčrtanim primerom izpustil — videti je kot naključen zapis ali oznaka, ne del besedila.
- Drevesni diagram sem narisal po smislu; tvoja skica je razvejana drugače in jo je morda treba popraviti.
- $F_{\text{state}}$ je na koncu strani 2 omenjen brez konteksta — domnevam, da je to napoved naslednje teme.