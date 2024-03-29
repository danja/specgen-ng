# SpecGenNG

**Yet Another Version of SpecGen!**

Documentation generator for RDF/OWL vocabularies/ontologies : RDF + template in, HTML out.

This is [zazi0815](https://smiy.wordpress.com/2010/07/13/my-specgen-version-6/)'s Version 6, ported to **Python 3** (from 2.6).

Notes under [About v6](https://github.com/danja/specgen-ng#about-v6) are lifted from https://github.com/specgen/specgen, will mostly apply here too. Some notes about porting in [here](https://github.com/danja/specgen-ng/blob/main/danny-notes.md).

I'll update instructions here once I've actually used it properly and someone else has kicked its tyres (and time permits). The command line below is fairly self-explanatory.

Key bits are:

- clone this repo
- put your vocab somewhere convenient (like the `onto` dir)
- `cd specgenng`
- run something like :

```
python src/specgenng.py \
      --indir=onto/project/ \
      --ns=http://purl.org/stuff/project/ \
      --prefix=prj \
      --ontofile=project_2007-09-03.rdf \
      --templatedir=template \
      --outdir=spec/project/ \
      --outfile=project_2007-09-03.html
```

A template file is required, there's a blank one under `template`.

_I wanted to update a vocab+docs and after spending some serious time hunting for a recent version of specgen or equivalent, gave up, bit the bullet, updated this v6. The heavy lifting was done by Python 2to3. I've named it SpecGenNG (v1.0.0 I guess) because it's a breaking change and there's probably a v7 already out there... There was no obvious license so I've slapped an MIT on it just in case, but to all intents and purposes it's public domain._

---

![Zombie foafsters](https://github.com/danja/specgen-ng/blob/main/template/zombie-foafsters.jpg)

---

## About v6 (2010-ish)

This is an experimental new codebase for [specgen](http://forge.morfeo-project.org/wiki_en/index.php/SpecGen) tools based on danbri's [specgen5 version](http://svn.foaf-project.org/foaftown/specgen/),
which was heavily updated by [Bo Ferri](http://github.com/zazi/) in summer 2010.

<b>Features (incl. modifications + extensions)</b>:

- multiple property and class types
- muttiple restrictions modelling
- rdfs:label, rdfs:comment
- classes and properties from other namespaces
- inverse properties (explicit and anonymous)
- sub properties
- union ranges and domains (appear only in the property descriptions, not on the class descriptions)
- equivalent properties
- simple individuals as optional feature

## Dependencies

It depends utterly upon

- [rdflib](http://rdflib.net/)
- [rdfextras](http://code.google.com/p/rdfextras/) (`easy_install rdfextras`)
- [pyparsing](http://pyparsing.wikispaces.com/) (`easy_install pyparsing`)
- [igraph](http://igraph.org/python/) (`easy_install python-igraph`)

(at least I had to install these packages additionally ;) )

If you're lucky, typing this is enough:

    easy_install rdflib python-igraph

and if you have problems there, update easy_install etc with:

    easy_install -U setuptools

Ubuntu, you can install the dependencies with pip after installing the relevant libraries

```
sudo apt-get install python-dev build-essential libxml2-dev libxslt python-igraph
sudo pip install -r requirements.txt
```

## Purpose

<b>Inputs</b>: [RDF](http://www.w3.org/TR/rdf-primer/), HTML and [OWL](http://www.w3.org/TR/owl-semantics/) description(s) of an RDF vocabulary<br/>
<b>Output</b>: an [XHTML+RDFa](http://www.w3.org/TR/rdfa-syntax/) specification designed for human users

## Example

    specgen6.py --indir=onto/olo/ --ns=http://purl.org/ontology/olo/core#  --prefix=olo --ontofile=orderedlistontology.owl --outdir=spec/olo/ --templatedir=onto/olo/ --outfile=orderedlistontology.html

- the template of this example can also be found in the folder: onto/olo
- the output of this example can also be found in the folder: spec/olo

See [libvocab.py](https://github.com/specgen/specgen/blob/master/libvocab.py) and [specgen6.py](https://github.com/specgen/specgen/blob/master/specgen6.py) for details.

## Status

- we load up and interpret the core RDFS/OWL
- we populate Vocab, Term (Class, Property or Individual) instances
- able to generate a XHTML/RDFa ontology specification with common concepts and properties from OWL, [RDFS](http://www.w3.org/TR/rdf-schema/), RDF

## Known Forks

- [WebID fork](http://dvcs.w3.org/hg/WebID/file/029f115c08a5/ontologies/specgen) (note this link is only a reference to a specific revision of that fork; to ensure that you'll utilise the most recent one, go to summary and walk that path to the specgen directory again from the most recent revision ;) )

## TODO

- enable more OWL features, especially an automated construction of owl:Ontology (currently this must be done manually in the template)
- enable more support for other namespaces (super classes and super properties from other namespaces already possible)
- restructure the code !!!
- write a cool parser for the "\n"'s and "\t"'s etc. in the parsed comments (e.g. "\n" to \<br\/\> ...)

## Known Issues

<ol>
	<li>librdf doesn't seem to like abbreviations in FILTER clauses.

<ul>
	<li>this worked:
<pre><code>q= 'SELECT ?x ?l ?c ?type WHERE { ?x rdfs:label ?l . ?x rdfs:comment ?c . ?x a ?type .  FILTER (?type = &lt;http://www.w3.org/2002/07/owl#ObjectProperty\&gt;)  } '</code></pre></li>
<li>while this failed:
<pre><code>q= 'PREFIX owl: <http://www.w3.org/2002/07/owl#> SELECT ?x ?l ?c ?type WHERE { ?x rdfs:label ?l . ?x rdfs:comment ?c . ?x a ?type .  FILTER (?type = owl:ObjectProperty)  } '</pre></code>
(even when passing in bindings)</li>
<li>This forces us to be verbose, ie.
<pre><code>q= 'SELECT distinct ?x ?l ?c WHERE { ?x rdfs:label ?l . ?x rdfs:comment ?c . ?x a ?type . FILTER (?type = &lt;http://www.w3.org/2002/07/owl#ObjectProperty&gt; || ?type = &lt;http://www.w3.org/2002/07/owl#DatatypeProperty&gt; || ?type = &lt;http://www.w3.org/1999/02/22-rdf-syntax-ns#Property&gt; || ?type = &lt;http://www.w3.org/2002/07/owl#FunctionalProperty&gt; || ?type = &lt;http://www.w3.org/2002/07/owl#InverseFunctionalProperty&gt;) } '</pre></code></li>
</ul></li>	
<li>TODO: work out how to do ".encode('UTF-8')" everywhere</li>
<li>Be more explicit and careful re defaulting to English, and more robust when multilingual labels are found.</li></ol>

## PS

The [old project repository location at SourceForge](http://smiy.svn.sourceforge.net/viewvc/smiy/specgen/) is now deprecated. All new developments will be pushed to this repository location here at GitHub.
