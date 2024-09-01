# Zanzibar SpiceDB-like Reader +  Archimate PlantUML Generation Code  in less than 650 lines of golang : part III

This part follows parts I and II from Zanzibar SpiceDB-like Reader

This time, I wanted to integrate 2 features:


# Feature 1 : the “Subject Relations”

This feature is described in the zanzibar documentation.

The spicedb/authzed documentation explains :

> Relations can also "contain" references to other relations/permissions.
> For example, a group's member relation might include the set of objects marked as member 
> of another group, indicating that the other group's members are, themselves, members of this group:


So I changed the grammar from part 2 as follows, to be able to modify the lexical analysis.

# BNF grammar

```
// Zanzibar restricted EBNF grammar
// SpiceDB like
// Only relations are declared (not permissions)

<Zschema> ::= <Zdef>*
<Zdef> ::= "definition" <Zname> "{" <Zbody> "}"  ---> generation
<Zname> ::= <identifier>
<Zbody> ::= <Zrelation>*
<Zrelation> ::= "relation" <Rname> ":" <Sname> ("|" <Sname)*   ---> generation 
<Rname> ::= <identifier> 
<Sname> ::= <Zname> | <Zname> "#" <Rname>
<identifier> ::= [a-zA-Z_][a-zA-Z0-9_]*

```

# Example  with feature 1

With zschema5.zed like this, feature 1 is in group's member relation

![zschema](images\zedschema5.png)

# Feature 2 : 

To understand the search reverse from an object to an user (see my first article about zanzibar and prolog with "who can permission object ?" ) 

I draw 
- the relationships between an object and a user with a solid line (relationship is METADATA)
- the possible existence to go back to a user  by an oriented dotted arrow pointing towards the user.(made possible by the existence of tuples of DATA like object#relation@user or object#relation@group#user)

<span style="color:yellow">tape :</span> go run zreader.go -fschema "./zschema5.zed" -out "zschema5"

<span style="color:yellow">response: </span>

![response](images\resp1.png)

Plantuml allows you to generate the following PNG file with the generated zschema5.puml as input:

![example](images\zschema5.png)

# Example 2

Even if the parser detects errors, it tries to draw what it can

The schema is :

![example](images\zedschema6.png)

<span style="color:yellow">tape :</span> go run  zreader.go -fschema "./zschema6.zed" -out "zschema6"

<span style="color:yellow">response:</span>

![response](images\resp2.png)

Using the generated zedschema6.png file with PlantUML helps us to show the Archimate following diagram :

![example](images\zschema6.png)

# Help mode

<span style="color:yellow">tape :</span> go run zreader.go -help

#

About Zanzibar : https://storage.googleapis.com/pub-tools-public-publication-data/pdf/0749e1e54ded70f54e1f646cd440a5a523c69164.pdf

About SpiecDB : https://authzed.com/blog/spicedb-is-open-source-zanzibar#everybody-is-doing-zanzibar-how-is-spicedb-different

About PlantUML : https://plantuml.com/fr/download
