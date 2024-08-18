---

**Prerequisite:**  
Acquire the ATLAS-toolkit, for example, into your home directory:

```bash
git clone https://github.com/atlas-nano/ATLAS-toolkit.git
```

**First replicate to achieve desired dimension:**

```bash
~/ATLAS-toolkit/scripts/replicate.pl -b ~/ATLAS-toolkit/scripts/dat/elements_bgfs/Si-Silicon.bgf -d "8 8 8" -s bulkSi.bgf
```

**Then fix the bonds:**

```bash
~/ATLAS-toolkit/scripts/bondByDistance.pl -b bulkSi.bgf -s bulkSi.bonded.bgf
```

**Then cut it to the desired size using the following script.**  
The value after the `<` sign sets the radius of your NP in angstroms. I recommend you use as small of an NP as is possibly reasonable.

```bash
~/ATLAS-toolkit/scripts/getBGFAtoms.pl -b bulkSi.bgf -o "sqrt((xcoord-21.4)**2+(ycoord-21.4)**2+(zcoord-21.4)**2) < 20" -s siNP-4nm.bgf
```

I cleaned up the fields of the atoms in the original BGF files starting from the bare NP structure using `sed`. I named my NP (QDA) but you could choose a different 3-letter code.

```bash
cat siNP-4nm.bgf | sed 's/RES A   444/QDA A     1/' > test.bgf
```

**Add appropriate forcefield types:**

```bash
~/ATLAS-toolkit/scripts/autoType.pl -i test.bgf -f ~/ATLAS-toolkit/ff/DREIDING2.21.ff -s siNP-4nm.typed.bgf
```

For my NPs, I wanted to functionalize them with larger ligands, so I prepared files for that accordingly. From this point on, you will need to adapt this procedure to suit your system.

I functionalized the NP with oleic acids. Because the surface area of a 4nm diameter sphere is about 50nm², and the grafting density is about 2 ligands/nm², I use 100 ligands for the additive structure. I will use 93 ligands for the PXL structure to approximate the 13:1 ligand.

```bash
~/ATLAS-toolkit/scripts/functionalize.pl -f siNP-4nm.typed.bgf -g ../ligand-structures/oleic-acid.dreid.bgf -a "numbonds<4" -n 100 -c 20 -r 1 -s siNP-preadditive-oleic.bgf
```

```bash
cat siNP-preadditive-oleic.bgf | sed 's/RES A   444/OLC A     2/' > test.bgf
```

```bash
mv test.bgf siNP-preadditive-oleic.bgf
```

--- 
