sudo apt-get install python-dev build-essential libxml2-dev libxslt python-igraph
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
Note, selecting 'python-dev-is-python2' instead of 'python-dev'
E: Unable to locate package libxslt
E: Unable to locate package python-igraph

Of course, it's Python 2

.pydevproject is for Eclipse, yup, Python 2.6

only 3 .py files, worth a try with 2to3
mv to src-python2

pip install 2to3

2to3 --output-dir=src -W -n src-python2

looks promising, output has print() statements

OS dependencies, looking in synaptic, seems like:

libxslt -> libxlt1.1

python-igraph, there is a libigraph3 in Ubuntu distro, but first -

- requirements.txt

```
rdflib
rdfextras
pyparsing
python-igraph
```

https://python.igraph.org/en/stable/

pip install igraph
pip install rdflib
pip install pyparsing

ok

pip install rdfextras

found:
"In RDFLib >= 4.0, rdfextras is no longer required, SPARQL support and the utilities from this project are included in core."
thanks gromgull !

specgen6.py --indir=onto/olo/ --ns=http://purl.org/ontology/olo/core# --prefix=olo --ontofile=orderedlistontology.owl --outdir=spec/olo/ --templatedir=onto/olo/ --outfile=orderedlistontology.html

python src/specgenng.py --indir=onto/project/ --ns=http://purl.org/stuff/project/ --prefix=prj --ontofile=project_2007-09-03.owl --outdir=spec/project/ --outfile=project_2007-09-03.html

project_2007-09-03.owl

running, ew.

TabError: inconsistent use of tabs and spaces in indentation

VSCode package Python Indent - nah, just used copy/paste

Usage: src/specgenng.py --indir=dir --ns=uri --prefix=prefix [--outdir=outdir] [--outfile=outfile] [--templatedir=templatedir] [--indexrdf=indexrdf] [--ontofile=ontofile]

python src/specgenng.py --indir=onto/project/ --ns=http://purl.org/stuff/project/ --prefix=prj --ontofile=onto/project/project_2007-09-03.owl --outdir=spec/project/ --outfile=project_2007-09-03.html

Can't open onto/project/project_2007-09-03.owl in onto/project/

python src/specgenng.py --indir=onto/project/ --ns=http://purl.org/stuff/project/ --prefix=prj --outdir=spec/project/ --outfile=project_2007-09-03.html

python src/specgenng.py --indir=onto/project/ --ns=http://purl.org/stuff/project/ --prefix=prj --ontofile=project_2007-09-03.rdf --outdir=spec/project/ --outfile=project_2007-09-03.html
