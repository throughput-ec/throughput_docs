########
KEYWORDs
########

A ``KEYWORD`` can be associated with any ``ANNOTATION``.  A common use would be to
help discover an ``OBJECT`` associated with a keyword such as *climate* or *cheese*.
Keywords could be associated with an ``AGENT`` or any other node type, but a ``KEYWORD`` is always connected through an ``ANNOTATION``.
In this way, a ``KEYWORD`` may be associated with another ``KEYWORD`` through a synonymy which would be represented as indicated below.

FIGURE WITH SYNONYMY

************
SYNONYMY
************

Synonymy is indicated by a relationship ``isSynonym`` connected by an ``ANNOTATION``.  This format allows us to have a ``TextBody`` ``OBJECT`` that contains information about why the synonymy was generated, and the agent who generated that synonymy.

.. raw:: html

    <img src="../_static/keyword_synonym.svg" width=30%>

In this case we can match by:

.. code-block:: Cypher

  MATCH (k:KEYWORD {keyword: $userkw})
  OPTIONAL MATCH (syn:KEYWORD)<-[:isSynonym]-(:ANNOTATION)-[:isSynonym]->(k)
  WITH COLLECT(syn) + COLLECT(k) AS kws
  MATCH (kws)<-[:hasKeyword]-(:ANNOTATION)-[:Target]->(ob:OBJECT)

This will look for all keywords and keyword synonyms, and then find all objects associated with them.
