---
# try also 'default' to start simple
theme: seriph
class: 'text-center'
transition: slide-left
title: Quantum in Biology — Case Studies
layout: cover
comark: true
---

<div class="h-full w-full grid place-items-center">
  <div class="max-w-3xl mx-auto">
    <div class="text-sm tracking-[0.4em] uppercase text-[#7b6dff] opacity-80">Western Quantum Club</div>
    <h1 class="mt-4 text-6xl leading-tight font-semibold">Quantum in Biology</h1>
    <div class="mt-3 text-2xl text-[#b8f6dc]">Foundations + Case Studies</div>
  </div>
</div>

<div class="absolute inset-0 pointer-events-none">
  <!-- Top cloud -->
  <div class="absolute top-8 left-20 text-4xl opacity-60 text-[#7bd3ff] i-carbon:microscope animate-float-slow"></div>
  <div class="absolute top-10 left-1/3 text-3xl opacity-40 text-[#7bd3ff] i-carbon:ibm-cloud animate-float-slow"></div>
  <div class="absolute top-10 right-28 text-4xl opacity-60 text-[#7b6dff] i-carbon:chemistry-reference animate-float"></div>
  <div class="absolute top-12 right-1/3 text-3xl opacity-35 text-[#ff7bd4] i-carbon:graph-aggregator animate-float-slow"></div>

  <!-- Side cloud -->
  <div class="absolute top-1/4 left-14 text-3xl opacity-40 text-[#7bd3ff] i-carbon:schematics animate-float"></div>
  <div class="absolute top-1/3 right-14 text-3xl opacity-40 text-[#2ee6a6] i-carbon:tree animate-float-slow"></div>
  <div class="absolute top-1/2 left-14 text-3xl opacity-35 text-[#7bd3ff] i-carbon:wave-period animate-float"></div>
  <div class="absolute top-1/2 right-14 text-3xl opacity-35 text-[#2ee6a6] i-carbon:network-1 animate-float"></div>

  <!-- Bottom cloud -->
  <div class="absolute bottom-20 left-20 text-4xl opacity-50 text-[#2ee6a6] i-carbon:dna animate-float-slow"></div>
  <div class="absolute bottom-18 right-20 text-4xl opacity-50 text-[#ff7bd4] i-carbon:chemistry animate-float"></div>
  <div class="absolute bottom-12 left-1/3 text-3xl opacity-30 text-[#7b6dff] i-carbon:chart-scatter animate-float"></div>
  <div class="absolute bottom-12 right-1/3 text-3xl opacity-30 text-[#7bd3ff] i-carbon:chip animate-float-slow"></div>
  <div class="absolute bottom-6 left-1/2 -translate-x-1/2 text-3xl opacity-35 text-[#7b6dff] i-carbon:earth-americas animate-float"></div>
</div>

<style>
@keyframes floatUpDown {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-20px); }
}

@keyframes floatUpDownSlow {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-14px); }
}

.animate-float {
  animation: floatUpDown 6s ease-in-out infinite;
}

.animate-float-slow {
  animation: floatUpDownSlow 8s ease-in-out infinite;
}
</style>

---
layout: default
---

# Today

- Quick primer on key quantum chemistry terms
- Case studies on molecule simulations
  - IBM
  - Google
  - Rigetti Computing
- Why these results matter for biology
  - Vision
  - Enzymes
  - Photosynthesis

---
layout: center
class: text-center
---

# Background
## Terminology + Demos

---
layout: default
---

# Key Terms

<div class="flex flex-col gap-4 mt-4 text-xl">
  <GlossaryTerm term="Hamiltonian" definition="The energy operator for a system; it encodes kinetic + potential energy (H = T + V)." />
  <GlossaryTerm term="Ansatz" definition="A parameterized quantum circuit used to build trial states for VQE." />
  <GlossaryTerm term="Coherence" definition="The phase relationship between quantum states that enables interference." />
  <GlossaryTerm term="QPE" definition="Quantum phase estimation: finds eigenvalues of a unitary operator; precise but deep circuits." />
  <GlossaryTerm term="QFT" definition="Quantum Fourier transform: extracts phase/frequency information from a quantum state." />
</div>

<!--
- **Hamiltonian (H):** the energy operator for a molecule, comprised of kinetic plus potential energies
- **Ansatz:** contains adjustable parameters which control the quantum state the circuit prepares. German for "starting assumption"
- **QPE (quantum phase estimation):** a quantum algorithm used to determine the eigenvalue (phase) associated with an eigenvector of a unitary operator. Works by applying controlled powers of the unitary operation and using the quantum Fourier transformation to extract the phase encoded in the quantum state. It can be more precise but is harder on noisy hardware
- **QFT (quantum Fourier transform):** a quantum version of the classical Fourier transform which is used to extract frequency or phase information from a quantum state that would otherwise be hidden
-->

---
layout: center
class: text-center
---

# VQE (Variational Quantum Eigensolver)

- VQE is a hybrid quantum-classical algorithm used to estimate ground state energy
- The molecule's Hamiltonian is mapped to qubits

<img src="/images/vqe.png" class="mt-4 mx-auto w-4/5 max-h-80 object-contain" alt="Variational Quantum Eigensolver" />
<div class="text-center text-sm mt-2 text-white/80">
  Figure: VQE loop. A parameterized circuit is optimized to minimize the measured energy.
</div>

<!--
- Variational Quantum Eigensolver is a hybrid quantum-classical algorithm used to estimate the ground state energy of quantum systems (especially molecules)
- The molecule's Hamiltonian ($H=T+V$) is mapped to qubits so it can be representated as a quantum circuit
1. A squid has many arms so it represents the ansatz becuase it is controlling many parameters at once
2. Then we run the circuit on a quantum computer. It produces a quantum state q_f, which is our current guess for the molecule's ground state
3. The quantum computer measures expectation values of the Hamiltonian H_{target} to compute the energy. This energy acts as a loss function we want to minimize
4. Lastly, a classical optimizer takes the measured energy and adjusts circuit parameters \theta to try to lower the energy
- We repeat this circuit until convergence (when the energy stops decreasing). This gives an approximation of the ground state energy
-->

---
layout: center
class: text-center
---

# Interactive: VQE Loop
<VQESim />

---
layout: default
class: text-left
---

# Interactive: VQE vs QPE

<AlgoToggle />

---
layout: default
class: text-left
---

<div class="h-full w-full grid place-items-center">
  <div>
    <h1 class="text-4xl">Previous quantum studies in biology</h1>
    <div class="flex gap-8 items-center mt-4 mb-4 justify-center">
      <img src="/images/logos/google.svg" alt="Google logo" class="h-8" />
      <img src="/images/logos/ibm.svg" alt="IBM logo" class="h-8" />
      <img src="/images/logos/rigetti.svg" alt="Rigetti logo" class="h-7" />
    </div>
  </div>
</div>

---
layout: default
---

# Google: $H_2$ Energy Surface (2016)

- Quantum computers can estimate molecular ground state energies, which determine bonding, stability, and reactions<sup>2</sup>
- Demonstrates both VQE and phase estimation on a superconducting processor<sup>2</sup>
- This is a proof-of-concept step toward simulating complex biologcal molecules like proteins, enzymes, and drug compounds in the future<sup>2</sup>

<img src="/images/google_h2_2016_physorg.jpg" class="mt-4 mx-auto w-2/3 max-h-64 object-contain" alt="H2 energy curve and errors from O’Malley et al. (2016)" />

<div class="text-center">
  Figure: H<sub>2</sub> energy curve and errors (O’Malley et al., 2016).<sup>4</sup>
</div>

<!--
- In 2016, researchers at Google used a quantum processor to simulate the hydrogen moleucle and compute its energy surface
- The goal was to calculate how the energy of the molecule changes as the distance between the 2 atoms changes using both variational quantum eigensolver and quantum phase estimation
- This experiment used quantum phase estimation, which can estimate the eigenvalues of a Hamiltonian and therefore was mapped onto qubits so the quantum computer could simulate the behaviour of the electrons in the molecule
- By running the algorithm for different interatomic distances (which is the distance between 2 atoms), the researchers were able to calculate the molecule's potential energy surface
- The lowest point of this surface (shown on the x-axis) corresponds to the equilibrium bond length, which is where the molecule is most stable
- The y-axis shows the total energy of the molecule
- On the left graph, the black curve is the exact theoretical energy calcualted using classic quantum chemistry methods
- The red points are results from variation quantum eigensolver experiemnts
- The blue points are from the quantum phase estiamtion experiments run on a quantum processor
- The right graph shows the error in the energy calculation for different bond lengths
- That horizontal line represents the chemical accuracy threshold, which is the level of accuracy needed for useful chemistry predictions
- The red line is the variational quantum eigensolver, which shows that the errors are relatively small across most bond lengths since they're often below the chemical accuracy line
- THe green dots are phase estimation and they show larger errors due to noise and experimental limitations
- The results matched theoretical chemistry calculations, showing that a quantum computer could reproduce known molecular energy values
- This experiment was an early demonstration that quantum computers can simualate molecular systems, which is important for future applications in chemistry, materials science, and drug discovery
-->

---
layout: default
---

# Interactive: H₂ Energy Curve

<H2CurveSim />

---
layout: default
class: text-left
---

# Interactive: Noise vs. Energy

<NoiseSim />

---
layout: default
class: text-left
---

# IBM: VQE on Small Molecules (2017)

- Useful for understanding how molcules form bonds and react<sup>1</sup>
- Knowing the ground state of mleucles is critic al in fields like drug discovery, materials science, and biochemistry<sup>1</sup>
- This is a proof-of-concept step toward simulating complex biologcal molecules like proteins, enzymes, and drug compounds in the future<sup>1</sup>

<img src="/images/ibm_vqe_2017_fig3.jpg" class="mt-4 mx-auto w-2/3 max-h-64 object-contain" alt="Molecular energy curves for LiH and BeH2" />

<div class="text-center">
Figure: molecular energy curves for small molecules (Kandala et al., 2017).<sup>1</sup>
</div>

<!--
- In 2017, IBM researchers used the variational quantum eigensolver on a real superconducting quantum computer to simulate the Hydrogen molecule
- The goal was to compute the molecule's ground state energy, which determeines how stable the molecule is and how it behaves chemically
- They mapped the molecule's electronic Hamiltonian (which describes all electrons in a molecule) onto qubits and used VQE to find the energy
- The graphs show the the interatomic distance vs the energy
- The curve represents the potential energy surface of the molecule, with the lowest point on the curve representing the equilibrium bond length
- The dots are from the quantum computer, and it shows that it closely follows the theretical curve
- Google's experiemnt showed that quantum computers can reproduce the energy curve of a molecule using phase estimation, while IBM's experiment showed that hybrid algorithms like variaiton quantum eigensolver can do similar chemistry simulations more effectively on noisy hardware
- This was one of the first demonstrations showing that quantum computers can perform real quantum chemistry calculations, even with only a few qubits
-->

---
layout: default
---

# Rigetti Computing: Chemistry Benchmarking (2019)

<div class="grid grid-cols-2 gap-6 items-start">
  <div>
    <ul class="space-y-2">
      <li>Demonstrated molecular energy estimation on a superconducting quantum processor<sup>3</sup></li>
      <li>Tested chemically realistic molecules (NaH, KH, RbH)<sup>3</sup></li>
      <li>Experimental results closely matches theoretical chemistry calculations<sup>3</sup></li>
      <li>Showed that near-term quantum hardware can perform useful chemistry simulations<sup>3</sup></li>
      <li>Important benchmark for scaling quantum chemistry algorithms<sup>3</sup></li>
    </ul>

  </div>
  <div class="flex flex-col items-center">
    <img src="/images/rigetti_benchmark_2019_fig2.png" class="w-full max-w-sm max-h-[410px] object-contain" alt="Energy curves for NaH, KH, RbH benchmarking (McCaskey et al., 2019)" />
    <div class="mt-2 text-sm text-white/80 text-center">
      Figure: energy curves for NaH, KH, RbH benchmarking (McCaskey et al., 2019).<sup>3</sup>
    </div>
  </div>
</div>

<!--
- Rigetti Computing devlops superconducting quantum integrated circuits for quantum computers
- They used variational quantum eigensolver to compute molecular ground state energies
- They tested moelcules like sodium hydride, potassium hydride, and rubidium hydride on their Rigetti Computing superconducting quantum processor
- The reason for doing hydrides is becuase they have relatively few electrons (making the simulation small enough to run with limited qubits), they are "chemically realistic molecules" which just means they aren't simplified molecules that are used just for testing algorithms, and they have increasing complexity (going down the alkali metals) making it easy to see how the algorithm scales
- This demonstrated that near-term quantum computers (computers we expect to have in the near future) can perform accurate molecular energy estimation
- Again, it's a proof-of-concept step towards scalabale quantum chemistry simulations
-->

---
layout: default
---

# Quantum in Nature

- Many biological processes depend on molecular structure and energy (bonding, stability, and reaction pathways)
<div class="flex flex-col gap-3 mt-3">
  <GlossaryTerm term="Vision" definition="Retinal photoisomerization (11‑cis → all‑trans) in rhodopsin is an ultrafast, energy‑driven reaction that initiates sight.<sup>5</sup>" />
  <GlossaryTerm term="Enzymes" definition="Quantum‑chemical models of chorismate mutase show how enzymes lower reaction barriers by stabilizing transition states.<sup>6</sup>" />
  <GlossaryTerm term="Photosynthesis" definition="Energy transfer in the FMO complex shows quantum coherence effects that influence efficiency.<sup>7</sup>" />
</div>

- Today’s demos (IBM/Google/Rigetti) are small, but they establish the workflow needed for larger biological systems

<!--
- Many biological processes depend on moelcular structure and energy because at the most fundamental level, biology is driven by chemistry
- Chemical bonds determine how atoms form molecules and what shapes those molecules take, whcih affects biological interactions like protein folding, druh binding, and how biomolecuels interact inside cells
- Molecular stability is what we just saw examples of
- Biological reactions occur along specific energy pathways that pass through transition states and understnading these pathways explains hwo enzymes accelerate and how complex biochemical processes proceed
- Vision begins with a moelcule called retinal inside the rhodopsin pigemnt in the retina of our eye
- When a photon hits retinal, it rapidly changes shape from 11-cis retinal to all-trans retinal, which is when the double bond rotates, changing the molecule from bent to straight, which activates a signaling cascade which ultimately produces an electrical signal that's sent to the brain where it's interpreted as vision
- Chorismate mutase is an enzyme invovled in amino acid synthesis
- Quantum chemical simulaitons of chorismate mutase have shown that enzymes stablize the transition state, lowering the reaction's energy barrier, which is ultimatley how enzymes are catalysts
- This is espeically useful in designing drugs
- The Fenna-Matthews-Olson complex is a sunlight harvesting system found in green sulfur bacteria living in low-light conditions
- Experiemnts suggest that quantum coherence may help the excitation energy explore multiple pathways and reach the reaction center more efficiently
-->

---
layout: center
class: text-left
---

# Resources

<div class="grid grid-cols-2 gap-4 text-sm leading-snug">
  <ol>
    <li>Kandala et al., *Nature* (2017) — VQE for small molecules. https://www.nature.com/articles/nature23879</li>
    <li>O’Malley et al., *PRX* (2016) — Scalable molecular energies. https://research.google/pubs/scalable-quantum-simulation-of-molecular-energies/</li>
    <li>McClean et al., *npj QI* (2019) — Chemistry benchmark. https://www.nature.com/articles/s41534-019-0209-0</li>
    <li>Phys.org (2016) — Image credit for PRX figure. https://phys.org/news/2016-07-scalable-quantum-simulation-molecule.html</li>
  </ol>
  <ol start="5">
    <li>Skopintsev et al., *Nature* (2023) — Ultrafast vision. https://www.nature.com/articles/s41586-023-05863-6</li>
    <li>Ranaghan et al., *Org. Biomol. Chem.* (2004) — Chorismate mutase QM/MM. https://doi.org/10.1039/B313759G</li>
    <li>Engel et al., *Nature* (2007) — Photosynthetic coherence. https://www.nature.com/articles/nature05678</li>
  </ol>
</div>
