# Tema 7, Inferență Bayesiană cu PyMC

Toate modelele folosesc `pm.sample(2000, tune=2000, chains=4, random_seed=42)`, adică 4 lanțuri x 2000 eșantioane = 8000 eșantioane a posteriori per model (plus 2000 de tuning per lanț).

## Exercițiul 1

Model: `Y | N ~ Hypergeometric(N, M=20, n=15)`, prior `N ~ DiscreteUniform(20, 200)`, observație `Y = 4`.

- **a/b)** Posteriorul lui `N` are media `≈ 103`, mediana `≈ 95` și HDI 95% `≈ [41, 179]`. Estimarea clasică `Mn/Y = 75` este aproape de mediană. Media a posteriori este mai mare pentru că posteriorul este asimetric la dreapta (coadă lungă).
- **c)** `P(N > 100 | Y = 4) ≈ 0.45`. Eșantionul mic (doar 4 marcați recapturați) nu poate discrimina între o populație mică și una mare.
- **d)** Distribuția predictivă descrie o observație viitoare și combină incertitudinea despre `N` cu aleatoriul extragerii hipergeometrice.

## Exercițiul 2

Model: timpi exponențiali cu priori `λ_A, λ_B ~ Gamma(2, 4)` (formă–rată).

- **a/b)** Timpul mediu `1/λ`: A `≈ 3.80` zile (HDI 95% `[1.72, 6.40]`), B `≈ 2.92` zile (HDI 95% `[1.29, 4.85]`), apropiate de mediile empirice `3.77` și `2.79`. Posterioarele se suprapun parțial (doar 8 observații/grup).
- **c)** `P(timp mediu B < timp mediu A | date) ≈ 0.72` compară mediile populațiilor.
- **d)** `P(pacient nou B < pacient nou A) ≈ 0.55` compară doi indivizi viitori. Este mai aproape de 0.5 pentru că include variabilitatea individuală a timpilor exponențiali, care se suprapun mult chiar când mediile diferă.

## Exercițiul 3
Model: `Y_t | λ_t ~ Poisson(λ_t)`, cu `λ_t` logistic între `λ_1` și `λ_2`; priori `λ_1, λ_2 ~ Exponential(0.2)`, `τ ~ Uniform(1, 30)`, `k ~ Exponential(1)`.

- **a/b)** `λ_1 ≈ 2.95` (nivel inițial), `λ_2 ≈ 9.8` (nivel final), `τ ≈ 15.9` (ziua centrală a tranziției, HDI 95% `[8.9, 23.0]`), `k ≈ 0.61` (viteză mică înseamnă tranziție graduală, HDI 95% `[0.04, 1.82]`).
- **c)** `P(λ_2 > λ_1 | Y) = 1.00`: datele susțin o creștere a ratei. Curba `λ_t` urmează panta datelor, de la `≈ 3` la `≈ 10` solicitări/zi.
- **d)** Aproape toate cele 30 de observații cad în banda predictivă de 95% (care include Poisson).