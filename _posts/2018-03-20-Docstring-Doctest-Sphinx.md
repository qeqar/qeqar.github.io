---
layout: post
title: Docstring, Doctest, Sphinx -> eine kleine Python Reise
tags: python docstring doctest sphinx dokumentation documentation
---

*Docstrings* habe ich schon früh in meiner Pythonreise kennengelern aber nie wirklich sinnvoll verwendet.

Meisten ist es nie über
```
def check_days(values):
    """ check each day if ok
    ok when no objectcount or objectcount == synccount
    """
```
drüberhinweggekommen.


Viel später bin ich über einen Kollegen auf *Doctests* gestoßen. Hier wird innerhalb des Docstrings Tests hinterlegt.

```
def check_days(values):
    """
    >>> import check
    >>> print check.check_days([{'bucket': 'bucket_name', 'date': '2018-01-01'}])
    (True, '2018-01-01: no objects to sync for bucket_name\\n')
    """
```
Wenn das Programm jetzt mit `python -mdoctest script.py` aufgerufen wird, wird nicht der Code ausgeführt sondern die Tests.
Wie aufmerksamen lesern aufgefallen sein könnte ist die Strucktur der Test identisch mit der Python Shell.
Somit können alle Test und exakte Outputs dort vorher manuell überprüft werden.

Was Doctest nicht kann (auf jedenfall habe ich keine Ahnung wie) ist Apis Mocken.
So wird man aber gezwungen die apicalls in anderes Funktionen wie die Logik zu legen.

```
def get_values():
    """ without doctest

    :return: array
    """

    return api_x.get_items()

def check_days(items):
    """ with doctest 

    :Example:

    >>> items = ['item1', 'item2']
    >>> print check_items(items)
    ("es waren 2 items)
    """

    return "es waren " + len(array) + " items"
```

Was sich als antipattern rausgestellt hat ist der expelziete Aufruf bei Classen

```
Class abc():


if __name__ == '__main__':
    import doctest
    ....
```
Bei Classen hat das noc funktioniert spätesten bei Programmen/Scripte nicht mehr.
Ausserdem hat man dann mindest in jeder Class 4 extra Zeilen Code, die überflüssig sind nur um sich das `-mdoctest` zu sparen :-)


Und dann kam *Sphinx* das Ergebniss hat jeder von uns wohl schon immer benutzt.
So ziemlich jede Pathon Dokumentation ist in Sphinx gerendert und in den meisten Fällen bei [readthedoc](https://readthedocs.org/) gehostet.


So Richtig zusammen gefunden hat das ganze erst als mit klar wurde das man Docstrings, Doctests und Sphinx super kombinieren kann.

Ich kann in Sphinx zusätzlich zu der Dokumentaion die ich extra schreibe, auch die Docstring Doku mit einlesen.

Diese kann man entgegen meines ersten bsp, noch massiv verfeinern
```
def check_days(values):
    """ check each day if ok
    ok when no objectcount or objectcount == synccount

    :param values: dict of dates/objectcount/synccount
    :type values: dict
    :return: boolen OK and message with all days
    """
```

Wenn wir dann noch eien Doctest mit hinterlegen und ein `:Example:` docstring davor packen haben wir alles in einer Welt.

```
def check_days(values):
    """ check each day if ok
    ok when no objectcount or objectcount == synccount

    :param values: dict of dates/objectcount/synccount
    :type values: dict
    :return: boolen OK and message with all days

    :Example:

    >>> import check
    >>> print check.check_days([{'bucket': 'bucket_name', 'date': '2018-01-01'}])
    (True, '2018-01-01: no objects to sync for bucket_name\\n')
    >>> print check.check_days([{'bucket': 'bucket_name', 'date': '2018-01-01', 'objectcount': 8, 'synccount' : 8}])
    (True, '2018-01-01: everything synced in bucket_name\\n')
    >>> print check.check_days([{'bucket': 'bucket_name', 'date': '2018-01-01', 'objectcount': 8, 'synccount' : 7}])
    (False, '2018-01-01: ERROR only 7 objects form 8 synced in bucket_name\\n')
    """
```

Wir haben einen Dokumentierten Code, einen Test und Sphinx kann aus der Kombination auch noch automagisch eine Module Dokumentation machen.

<img src="/images/sphinx.png" alt="generated Sphinx doku" class="inline"/>




**Damit war meine Dokumentations Liebe geboren!!!**

Was mir jetzt noch Fehlt ist der Allgemeine interne Dokuserver alla readthedocs.
Dort sollten alle Dokus die mit in den Pipelines mit generiert werden, hinterlegt werden.

aber den bekomme ich auch noch hin.
