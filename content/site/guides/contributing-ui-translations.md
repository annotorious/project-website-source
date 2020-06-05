---
title: "Contributing UI Translations"
date: 2020-06-01T20:04:18+02:00
draft: false
layout: "section-page"
subsection: "guides"
blurb: "We want to make our user interface available in as many languages as possible. If you want to help: contributing a translation is easy. This guide explains how."
weight: 2
---

# Contributing UI Translations

We want to make Annotorious available in as many languages as possible. If you want to help: providing a translation is easy - and because Annotorious doesn't have many labeled user interface elements, it's also not a lot of work.

## Message Dictionaries

The user interface labels are part of the `recogito-client-core` package, [located here](https://github.com/recogito/recogito-client-core/tree/master/src/i18n). In this folder you will find
`messages` files, one file for each available translation. 

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
- [Send us a pull request](https://www.freecodecamp.org/news/how-to-make-your-first-pull-request-on-github-3/)

Many thanks in advance! If you have questions, do get in touch via the [Annotorious chat on Gitter](https://gitter.im/recogito/annotorious). 

## Translations so far

| Locale | Language | Status | Dictionary file |
|--------|----------|--------|-----------------|
| ar | Arabic   | Partial   | [messages_ar.json](https://github.com/recogito/recogito-client-core/blob/master/src/i18n/messages_ar.json) |
| de | German   | Complete | [messages_de.json](https://github.com/recogito/recogito-client-core/blob/master/src/i18n/messages_de.json) |
| es | Spanish  | Partial  | [messages_es.json](https://github.com/recogito/recogito-client-core/blob/master/src/i18n/messages_es.json) |
| gl | Galician | Partial   | [messages_gl.json](https://github.com/recogito/recogito-client-core/blob/master/src/i18n/messages_gl.json) |
| hi | Hindi | Complete   | [messages_hi.json](https://github.com/recogito/recogito-client-core/blob/master/src/i18n/messages_hi.json) |
| it | Italian | Partial   | [messages_it.json](https://github.com/recogito/recogito-client-core/blob/master/src/i18n/messages_it.json) |
| sv | Swedish | Partial   | [messages_sv.json](https://github.com/recogito/recogito-client-core/blob/master/src/i18n/messages_sv.json) |
| ur | Urdu | Complete   | [messages_ur.json](https://github.com/recogito/recogito-client-core/blob/master/src/i18n/messages_ur.json) |

Your language isn't on this list? Consider contributing!

