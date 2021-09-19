---
title: "Contributing Translations"
date: 2020-06-01T20:04:18+02:00
draft: false
layout: "section-page"
subsection: "guides"
blurb: "We want to make the user interface available in as many languages as possible. If you want to help: contributing a translation is easy. This guide explains how."
weight: 98
meta_title: "Contributing UI Translations"
meta_description: "We want to make the user interface available in as many languages as possible. If you want to help: contributing a translation is easy. This guide explains how"
meta_link: "https://recogito.github.io/guides/contributing-ui-translations"
---

# Contributing UI Translations

We want to make the user interface available in as many languages as possible. If you want to help: providing a translation is easy - and because there are not many labeled elements, it's not a lot of work.

## Message Dictionaries

User interface labels are part of `recogito-client-core`, a JavaScript package for common code shared between
Annotorious and RecogitoJS. In [this folder](https://github.com/recogito/recogito-client-core/tree/master/src/i18n) 
you will find `messages` files, one file for each available translation. 

Each `messages` file is a dictionary of the default English labels and their translations. For example, 
here's what the German translation file `messages_de.json` looks like.

```json
{
  "Add a comment...": "Kommentar schreiben...",
  "Add a reply...": "Antwort schreiben...",
  "Add tag...": "Tag...",
  "Cancel": "Abbrechen",
  "Close": "Schliessen",
  "Edit": "Bearbeiten",
  "Delete": "Löschen",
  "Ok": "Ok"
}
``` 

## To add a new translation

- Fork the [recogito-client-core repository](https://github.com/recogito/recogito-client-core)
- Add a message file to the [src/i18n folder](https://github.com/recogito/recogito-client-core/tree/master/src/i18n) named `messages_{iso}.json`, where {iso} is the 2-character ISO code of
  the language.
- Copy the dictionary above, and replace the translations accordingly.
- [Send a pull request](https://www.freecodecamp.org/news/how-to-make-your-first-pull-request-on-github-3/)

Many thanks in advance! If you have questions, do get in touch via the [Annotorious chat on Gitter](https://gitter.im/recogito/annotorious). 

## Translations so far

| Language | Locale | Status | Dictionary file |
|--------|----------|--------|-----------------|
| Arabic | ar  | __Partial__ | [messages_ar.json](https://github.com/recogito/recogito-client-core/blob/master/src/i18n/messages_ar.json) |
| Czech | cs | Complete | [messages_cs.json](https://github.com/recogito/recogito-client-core/blob/master/src/i18n/messages_cs.json) |
| Dutch | nl | Complete | [messages_nl.json](https://github.com/recogito/recogito-client-core/blob/master/src/i18n/messages_nl.json) |
| Finnish | fi | Complete | [messages_fi.json](https://github.com/recogito/recogito-client-core/blob/master/src/i18n/messages_fi.json) |
| French | fr | Complete | [messages_fr.json](https://github.com/recogito/recogito-client-core/blob/master/src/i18n/messages_fr.json) |
| Galician | gl | Complete  | [messages_gl.json](https://github.com/recogito/recogito-client-core/blob/master/src/i18n/messages_gl.json) |
| German | de   | Complete  | [messages_de.json](https://github.com/recogito/recogito-client-core/blob/master/src/i18n/messages_de.json) |
| Greek | el   | Complete  | [messages_el.json](https://github.com/recogito/recogito-client-core/blob/master/src/i18n/messages_el.json) |
| Hindi | hi | Complete   | [messages_hi.json](https://github.com/recogito/recogito-client-core/blob/master/src/i18n/messages_hi.json) |
| Italian | it | __Partial__ | [messages_it.json](https://github.com/recogito/recogito-client-core/blob/master/src/i18n/messages_it.json) |
| Korean | ko | Complete | [messages_ko.json](https://github.com/recogito/recogito-client-core/blob/master/src/i18n/messages_ko.json) |
| Portugese | pt | Complete | [messages_pt.json](https://github.com/recogito/recogito-client-core/blob/master/src/i18n/messages_pt.json) |
| Spanish | es  | Complete  | [messages_es.json](https://github.com/recogito/recogito-client-core/blob/master/src/i18n/messages_es.json) |
| Swedish | sv | __Partial__ | [messages_sv.json](https://github.com/recogito/recogito-client-core/blob/master/src/i18n/messages_sv.json) |
| Turkish | tr | Complete | [messages_sv.json](https://github.com/recogito/recogito-client-core/blob/master/src/i18n/messages_tr.json) |
| Urdu | ur | Complete   | [messages_ur.json](https://github.com/recogito/recogito-client-core/blob/master/src/i18n/messages_ur.json) |

Your language isn't on this list? Consider contributing!

