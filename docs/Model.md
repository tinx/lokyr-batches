# Lokyr Batches - Entity Model

There are two types of batches:

* ingredient batches
* production batches

They are so different from each other that there is no common base class
or common code. For example, ingredient batches consist of exactly one type
of material, like honey or raspberries, while production batches will
contain a multitude of ingredients. Also, for ingredient batches we must
register their source and date of purchase, while for production batches
we can't do either.

## Ingredient Batches

We store these pieces of information per ingredient batch:

* lokyr batch name (e.g. "L2018-51")
* manufacturer batch name
* product (manufacturer and type, e.g. yeast, water, hazelnut aroma, ...)
* procurement source (whom did we buy it from?)
* procurement date
* procurement units (milliliters, kilograms, pieces, ...)
* procurement amount
* best before date
* goods inspector
* goods inspection date
* goods inspection result [note: more than one inspection?]
* invoice
* delivery note
* photos
* other documents
* arbitrary batch metadata (key/value)

## Production Batches

We store these pieces of information per production batch:

* lokyr batch name (e.g. "L2018-51")
* parent batch (e.g. a Batch-A of classic mead plus an armoma becomes Batch-B)
  (blends might even have several parent batches ...)
* product (strawberry mead, raspberry mead, ginger beer, ...)
* best before date
* fermentation start date
* racking date(s)
* filter date(s), including filter types used
* bottling date
* retained sample location(s)
* retained sample label(s)
* goods inspector
* goods inspection date
* goods inspection result [note: more than one inspection?]
* alcohol content per volume
* laboratory result reports
* photos
* other documents
* arbitrary batch metadata (key/value)

