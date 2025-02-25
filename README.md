# sig2lead2.0

## A

### All Targets from the DUD-E database [https://dude.docking.org/]
`3eml,2hzi,3bkl,1e66,2e1w,2oi0,2vt4,3ny8,3cqw,3d0e,2hv5,1l2s,2am9,1s3b,3l5d,3d4q,1bcd,2cnk,1h00,3bwm,1r9o,3nxu,3krj,3odu,1lru,3frj,2i78,3pbl,3nxo,2rgp,1sj0,2fsz,3kl6,1w7x,2nnq,3bz3,3c4f,1j4h,3e37,1zw5,3bqd,2v3f,3kgc,1vso,3max,3f07,3nf7,1xl2,3lan,3ccw,1uyg,3f9m,2oj9,4TRJ,2ica,3lpb,3cjo,3g0e,2b8t,2i0e,2of2,3chp,3m2w,2aa2,3lq8,2ojg,2zdt,2qd9,830c,3eqh,1qw6,1b9v,1kvo,3l3m,1udt,2oyu,3ln1,2owb,3bgs,2p54,2znp,2gtk,3kba,2azr,1njs,1c8k,1d3g,3g6z,2etr,1mv9,1li4,3el8,3hmm,1q4x,1ype,2ayw,2zec,1syn,1sqt,2p2i,3biz,3hl5`

* Note that 2h7l is renewed as 4TRJ

## B

### 1 Batch downloading `.fasta` files and `.pdb` files from the PDB database [https://www.rcsb.org/]
- a. Batch download `.fasta` files: https://www.rcsb.org/downloads/fasta
- b. Batch download `.pdb` files: https://www.rcsb.org/downloads

### 2 Batch downloading and processing Positive and Negative smis on DUDE



### 3 Generating 3Di with fold seek
- a. Setup foldseek (macOS)
```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
brew install wget
```

```
wget https://mmseqs.com/foldseek/foldseek-osx-universal.tar.gz
tar xvzf foldseek-osx-universal.tar.gz
export PATH=$(pwd)/foldseek/bin/:$PATH
```
* Linux: refers to this blog [https://blog.csdn.net/gitblog_01274/article/details/143039264]
- 

Split fasta files with this script:
```
from Bio import SeqIO
import os

input_fasta = "3di_embedding.fasta"
output_dir = "pdb_sequences"
Main_string = True

if not os.path.exists(output_dir):
    os.makedirs(output_dir)

for record in SeqIO.parse(input_fasta, "fasta"):

    if Main_string:
        if "_B" in record.id or "_C" in record.id or "_D" in record.id or "_E" in record.id or "_F" in record.id or "_L" in record.id or "_I" in record.id:
            continue

    pdb_id = record.id  # 使用PDB ID命名
    output_file = os.path.join(output_dir, f"{pdb_id}.fasta")

    with open(output_file, "w") as f:
        f.write(f">{record.id}\n{str(record.seq)}\n")

```


## C

### 1 Generating embeddings using PROST5


- for 3Di:
```
python scripts/embed_dir.py --input ./\(1\)3di_pdb_sequences\(each-mainstring\) --output ./\(1\)3di_pdb_sequences_emb --half 1 --is_3Di 1 --per_protein 1
```
- for AA:
```
python scripts/embed_dir.py --input ./\(2\)aa_pdb_fasta\(each-mainstring\) --output ./\(2\)pdb_fasta_emb --half 1 --is_3Di 0 --per_protein 1
```


