# Bioinformatics Algorithms

Python solutions to 23 problems from [Rosalind](https://rosalind.info), a site for learning bioinformatics by solving problems. The problems come from Rosalind's *Bioinformatics Stronghold* and *Bioinformatics Textbook Track*, and cover:

- **DNA basics:** GC content, reverse complement, k-mers, mRNA counting
- **Sequence alignment:** edit distance, global (affine gap), local, multiple alignment
- **Phylogeny (evolutionary trees):** distance matrices, limb lengths, additive phylogeny, UPGMA, neighbor joining, small parsimony
- **Genome assembly:** error correction, De Bruijn graphs, assembly from reads, N50/N75
- **String indexing:** trie, Burrows-Wheeler transform (BWT)

Each folder has one solution (`Q<number>.py`) and a screenshot (`.png`) of the problem statement, with its sample input and output.

## Problems

| # | Problem | Rosalind | Input |
|---|---|---|---|
| 1 | Computing GC Content | [GC](https://rosalind.info/problems/gc/) | file `rosalind_gc.txt` |
| 2 | Complementing a Strand of DNA | [REVC](https://rosalind.info/problems/revc/) | keyboard (stdin) |
| 3 | Enumerating k-mers Lexicographically | [LEXF](https://rosalind.info/problems/lexf/) | keyboard (stdin) |
| 4 | Construct a Trie from a Collection of Patterns | [BA9A](https://rosalind.info/problems/ba9a/) | file `rosalind_ba9a.txt` |
| 5 | Creating a Distance Matrix | [PDST](https://rosalind.info/problems/pdst/) | file `Q1.txt` |
| 6 | Edit Distance Alignment | [EDTA](https://rosalind.info/problems/edta/) | file `Q2.txt` |
| 7 | Global Alignment with Scoring Matrix and Affine Gap Penalty | [GAFF](https://rosalind.info/problems/gaff/) | files `Q3.txt` + `Q3_matrix.txt` (BLOSUM62) |
| 8 | Local Alignment with Scoring Matrix | [LOCA](https://rosalind.info/problems/loca/) | files `Q4.txt` + `Q4_matrix.txt` (PAM250) |
| 9 | Multiple Alignment | [MULT](https://rosalind.info/problems/mult/) | file `Q10.txt` |
| 10 | Inferring mRNA from Protein | [MRNA](https://rosalind.info/problems/mrna/) | file `Q11.txt` |
| 11 | Error Correction in Reads | [CORR](https://rosalind.info/problems/corr/) | file `Q12.txt` |
| 12 | Compute Distances Between Leaves | [BA7A](https://rosalind.info/problems/ba7a/) | file `Q1.txt` |
| 13 | Compute Limb Lengths in a Tree | [BA7B](https://rosalind.info/problems/ba7b/) | file `Q2.txt` |
| 14 | Implement AdditivePhylogeny | [BA7C](https://rosalind.info/problems/ba7c/) | file `Q3.txt` |
| 15 | Implement UPGMA | [BA7D](https://rosalind.info/problems/ba7d/) | file `Q4.txt` |
| 16 | Implement the Neighbor Joining Algorithm | [BA7E](https://rosalind.info/problems/ba7e/) | file `Q5.txt` |
| 17 | Implement SmallParsimony | [BA7F](https://rosalind.info/problems/ba7f/) | file `Q6.txt` |
| 18 | Constructing a De Bruijn Graph | [DBRU](https://rosalind.info/problems/dbru/) | file `Q1.txt` |
| 19 | Genome Assembly Using Reads | [GASM](https://rosalind.info/problems/gasm/) | file `Q2.txt` |
| 20 | Genome Assembly with Perfect Coverage | [PCOV](https://rosalind.info/problems/pcov/) | file `Q3.txt` |
| 21 | Assessing Assembly Quality with N50 and N75 | [ASMQ](https://rosalind.info/problems/asmq/) | file `Q4.txt` |
| 22 | Reconstruct a String from its Burrows-Wheeler Transform | [BA9J](https://rosalind.info/problems/ba9j/) | file `Q5.txt` |
| 23 | Implement BWMatching | [BA9L](https://rosalind.info/problems/ba9l/) | file `Q6.txt` |

**Note:** the input file names don't follow the problem numbers (for example, problem 5 reads `Q1.txt`). Use the name in the table.

## How to run

**You need:** Python 3. Only problem 9 needs an extra package:

```bash
pip install numpy
```

**Problems that read a file:**

1. Get the dataset (for example, download it from the problem's Rosalind page). Rosalind names its files like `rosalind_pdst.txt`.
2. Rename it to the name in the table above.
3. Put it in the folder you run the script from, then run the script. For example, for problem 5:

```bash
cd "5-Creating a Distance Matrix"
python Q5.py
```

**Problems 2 and 3** read from the keyboard. Type or paste the input and press Enter, or pass a file:

```bash
python Q2.py < rosalind_revc.txt
```

**Problems 7 and 8** also need a scoring matrix file (BLOSUM62 for problem 7, PAM250 for problem 8). Use the matrix linked from the Rosalind problem page, saved as a plain text table:
- one header line with the 20 amino acid letters,
- then 20 rows, each a letter followed by 20 numbers,
- letters in this order: `A C D E F G H I K L M N P Q R S T V W Y`.

**Problem 9** expects each sequence on a single line (the code doesn't join sequences split over several lines).

## Testing

All 23 solutions were run on the sample dataset shown in each problem's screenshot:

- **Correct:** problems 1–6 and 10–23.
- **Problems 17, 19 and 20** give a different answer than the sample, but Rosalind accepts any valid answer for them. Problem 17 gives a different labeling with the same score (16). Problems 19 and 20 give the same circular genome starting at a different point (or its reverse complement), and this can change between runs.
- **Problems 7, 8 and 9** have issues (see below).

## Known issues

- **Problem 7 (affine gap):**
  - The printed score is 2 too high. The sample gives `10` instead of `8`, because of `print(-1 * min_edit + 2)`.
  - On longer inputs, the alignment it finds is not always the best one.
- **Problem 8 (local alignment):** the score is correct, but it prints the full input strings instead of the aligned substrings (on the sample: the whole strings instead of `LYPRTEINSTRIN` and `LYEINSTEIN`).
- **Problem 9 (multiple alignment):** the score is correct (`-18` on the sample), but the four aligned strings it prints are wrong (reversed and mixed up).
- **Problems 6, 7 and 8 crash on long inputs** (Rosalind allows up to 1000 letters) with `RecursionError`. Fix: add these lines at the top of the script:

  ```python
  import sys
  sys.setrecursionlimit(10000)
  ```

- **Problem 1** shows a `SyntaxWarning` about `is` on newer Python versions. The result is still correct.

## Credits

- Problem 6 is based on [aladdinpersson's sequence alignment code](https://github.com/aladdinpersson/Algorithms-Collection-Python/blob/master/Algorithms/dynamic_programming/sequence_alignment.py).
- Problem 16 used [this neighbor-joining guide](https://www.tenderisthebyte.com/blog/2022/08/31/neighbor-joining-trees/).
- Problems and screenshots: [Rosalind](https://rosalind.info).
