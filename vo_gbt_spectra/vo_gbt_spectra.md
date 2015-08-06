[Back to Joe's Notes](http://www.cv.nrao.edu/~jmasters)

# VO GBT Spectra

* [Purpose](#purpose)
* [Uses](#uses)
* [Design](#design)
* [Implementation](#implementation)
* [Installation](#installation)
* [Running](#running)
* [Open issues](#tbd)
* [Other](#other)
* [References](#references)

## [Purpose](id:purpose)

### Problem

GBT spectra are not directly available to VO services.

### Solution

Create an archive of GBT spectral data products and a server to process SSAP queries from VO client tools.

## [Typical Uses](id:uses)

## [Design](id:design)


### Constraints

This service will not provide a comprehensive view of GBT spectra.  Instead, it will cover a curated subset of observations.  To keep the implementation process simple and predictable, we will not use the DAL Server or other VO frameworks.

## [Implementation](id:implementation)

### Timeline

1. Identify spectra that could be published (e.g. survey data form various GB scientists)

	**We will use Jim Braatz's GBT H2O maser data.  He is OK with this and the data is familiar from the spectral pipeline project.**


   Timeline:  ~2 weeks  [First week of May]

   Effort:  2 days

1. Identify clients that could already query GBT spectra via SSAP requests.
   In other words, if we register our GBT spectra with the VO directory service, the data become exposed to queries without any additional effort.

   Timeline:  ~2 weeks  [mid-May]

   Effort:  1 day

1. If clients do not exist, we need to develop our own.  This could be a simple web based query form.

	**We implemented a cone search query service by running a simply web.py python server that handles http GET requests and responds with a very basic VO table.  What we really want is a SSAP service, but this was a good proof of concept and potentially useful on its own.
	The code is here: [https://github.com/jmasters/gbt-vo.git](https://github.com/jmasters/gbt-vo.git)**
	
	**We identified the Datascope as a client to query for GBT spectra.  If it is a maintained tool, our products should eventually show up in its results.**
	
   Timeline:  1 week  [Start of June]

   Effort:   1 week

1. Place spectra and metadata in a "database", directory, etc.

	**We have a script (make_vo_lite_sqlite_db.py) that puts the spectra and metadata in a sqlite database (gbt_spectra.sqlite).**

   Timeline:  2 weeks [3rd week of June]

   Effort:  2 weeks

1. Provide VOTable responses to SSAP HTML GET requests

	**We need to add a timestamp for each spectrum to the database.  DATE-OBS?  SSAP requires: POS (ICRS), SIZE, BAND, TIME, FORMAT"**


   Timeline:  3 weeks  [2nd week of July]

   Effort: 3 weeks

1. Talk to systems / security group at NRAO to make sure security and data availability are optimal.

   Timeline:  1 week  [3rd week of July]

   Effort:  TBD

1. Testing of requests and responses

   Timeline:  1 week [4th week of July]

   Effort:  3 days

1. Register the data with the VO directory service

   Timeline:  1 day

   Effort:  1 day

**Total effort:**  7-8 weeks

**Total time:**  12 weeks

### Dependencies

## [Installation Instructions](id:installation)

Check out the code

```
git clone https://github.com/jmasters/gbt-vo.git
cd gbt-vo/
```

Create a link called "spectra", which points to a directory containing calibrated single spectrum SDFITS files.

```
ln -s <target-directory> spectra
```

If the database (gbt_spectra.sqlite) does not exist, create it

```
python make_vo_lite_sqlite_db.py
```



## [How to run](id:running)

Start the server

```
python cone_search_server.py 8888 &
```

Open a browser and query the server.  For example

```
http://beefy.cv.nrao.edu:8888?ra=6&dec=45&sr=10
```

## [Open issues](id:tbd)

It isn't easy to determine which VO specification is "the latest".  Google searches and various VO web sites reveal different versions to be the most up to date.  What is the definitive source?

## [Other useful things to know about](id:other)

## [References](id:references)

* SSAP Spec: [http://www.ivoa.net/documents/latest/SSA.html](http://www.ivoa.net/documents/latest/SSA.html)
* Cone Search Spec: [http://www.ivoa.net/documents/latest/ConeSearch.html](http://www.ivoa.net/documents/latest/ConeSearch.html)
* Datascope: [http://heasarc.gsfc.nasa.gov/cgi-bin/vo/datascope/init.pl](http://heasarc.gsfc.nasa.gov/cgi-bin/vo/datascope/init.pl)



