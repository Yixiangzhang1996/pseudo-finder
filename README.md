# Pseudo finder
Pseudo finder is a Python3 script that detects pseudogene candidates [https://en.wikipedia.org/wiki/Pseudogene] in annotated genbank files of bacterial and archaeal genomes. It was tested mostly on genbank (.gbf/.gbk) files annotated by Prokka [https://github.com/tseemann/prokka] with the --compliant flag.

There are alternative programs for pseudogene finding and annotation (e.g. the NCBI Prokaryotic Genome Annotation Pipeline [https://www.ncbi.nlm.nih.gov/genome/annotation_prok/]), but to the best of our knowledge, none of them is open source and allows fine-tuning of parameters.


## Authors
Mitch Syberg-Olsen & Filip Husnik

University of British Columbia, Vancouver, Canada

## Versions and changes


## Getting Started

These instructions will (hopefully) get you the pipeline up and running on your local machine.

### Prerequisites

Installation requirements: python3, pip3 (or any other way how to install python libraries), argparse, biopython, ncbi-blast+

Databases: NCBI-NR protein database (or similar such as SwissProt) formatted for blastp/blastx searches.

Input files: A genome sequence with gene annotations in the genbank (.gbf/.gbk) format.


### Installing

A step by step series of commands to install all dependencies:

Installation of python3, pip3, git, and ncbi-blast+ on Ubuntu:
```
sudo apt-get update

sudo apt-get install python3
sudo apt-get install python3-pip
sudo apt-get install ncbi-blast+
sudo apt-get install git
```

Installation of python3 libraries on Ubuntu:
```
sudo pip3 install argparse
sudo pip3 install biopython
```

Cloning the pseudo_finder.py code from github:
```
git clone https://github.com/filip-husnik/pseudo-finder.git
```

## Running pseudo finder

```
# Run the full pipeline on 16 processors (for BlastX/BlastP searches)
python3 pseudo_finder.py --genome GENOME.GBF --output PREFIX --database NR --threads 16 

# Run only pseudogene detection (e.g. when blast output files are already available from a previous run)
python3 pseudo_finder.py --genome GENOME.GBF --output PREFIX --blastp BLASTPFILE.TSV --blastx BLASTX.TSV
```

```
# All command line arguments.
'-g', '--genome', help='Please provide your genome file in the genbank format.', required=True

'-o', '--output', help='Specify an output prefix.', required=True

'-d', '--database', help='Please provide name (if $BLASTB is set on your system) or absolute path of your blast database.')

'-p', '--blastp', help='Specify an input blastp file.'

'-x', '--blastx', help='Specify an input blastx file.'

'-t', '--threads', help='Please provide total number of threads to use for blast, default is 4.'

'-i', '--intergenic_length', help='Please provide length of intergenic regions to check, default is 30 bp.'

'-l', '--length_pseudo', help='Please provide percentage of length for pseudo candidates, default is 0.60 (60%).'

'-s', '--shared_hits', help='Percentage of blast hits that must be shared in order to join two nearby regions, default is 0.30 (30%).'

'-e', '--evalue', help='Please provide e-value for blast searches, default is 1e-4.'
```

## Output of pseudo finder
Every run should result in two output files: a summary .log file and a .gff file.

(1) The .log file includes basic statistics about pseudogene candidates detected.

(2) The .gff file can be used to overlay the original annotation (we use the Artemis genome browser [http://www.sanger.ac.uk/science/tools/artemis]) with predicted pseudogene candidates.

## Contributing

We appreciate any critical comments or suggestions for improvements. Please raise issues or submit pull requests.

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details

## Acknowledgments

* This code was inspired by ...

## References

**Basic information about bacterial pseudogenes:**

Recognizing the pseudogenes in bacterial genomes: https://www.ncbi.nlm.nih.gov/pmc/articles/PMC1142405/

Taking the pseudo out of pseudogenes: https://www.ncbi.nlm.nih.gov/pubmed/25461580

**Several examples from the Sodalis clade showing how important is pseudogene annotation for bacteria in a nascent stage of symbiosis (or similar ecological shifts):**

Mobile genetic element proliferation and gene inactivation impact over the genome structure and metabolic capabilities of Sodalis glossinidius, the secondary endosymbiont of tsetse flies: https://www.ncbi.nlm.nih.gov/pubmed/20649993

A novel human-infection-derived bacterium provides insights into the evolutionary origins of mutualistic insect–bacterial symbioses: https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3499248/

Genome degeneration and adaptation in a nascent stage of symbiosis: https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3914690/

Repeated replacement of an intrabacterial symbiont in the tripartite nested mealybug symbiosis: https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5027413/

Large scale and significant expression from pseudogenes in Sodalis glossinidius - a facultative bacterial endosymbiont: https://www.biorxiv.org/content/early/2017/07/23/124388

## Wish list
These are several additional features we'll try to include in the script in the near future:

1. Check if the ORFs called as pseudogenes do not represent individual protein domains (incl. functional sites) that can exists and evolve independently of the rest of the original protein chain.
2. Include an optional analysis of cryptic pseudogenes based on dN/dS ratios (PAML?...) when there are closely related genomes available.
3. Include an optional analysis when there are RNA-Seq data available.
4. Fine tune pseudogen finding for mobile elements such as transposases.
5. Visualize results by a scatter plot of all genes/pseudogenes (dN/dS, GC content, length ratio, ...).
6. Sometimes ORFs are predicted by mistake on the opposite strand (e.g. in GC-rich genomes), check regions with ORFS with no blastp hits by blastx.

