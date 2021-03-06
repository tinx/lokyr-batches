# Lokyr Batches - Entity Model

A limited list of entity models is required. Since this is a micro-service,
we want the model to be independend of other databases. Thus, other data
sources might be referenced, but all information required to operate will
be retained as a copy local to this service. As a consequence, we must
always be prepared to handle broken references.

There are two types of batches:

* ingredient batches
* production batches

They are so different from each other that there is no common base class
or common code. For example, ingredient batches consist of exactly one type
of material, like honey or raspberries, while production batches will
contain a multitude of ingredients. Also, for ingredient batches we must
register their source and date of purchase, while for production batches
we can't do either.

# Entities

## Ingredient Batches

This is a batch of materials we use as input in our production. Examples
are containers of honey, fruits, chemicals or aromas. The aim here is
to register ingredients grouped by their "risk familiy". If, for example,
manufacturer X issues a recall on their product, we want to know which
ingredient batches are affected, and what they were used for.

We store these pieces of information per ingredient batch:

* lokyr batch name (e.g. "L2018-51")
* ingredient
* manufacturer batch name
* supplier (third party)
* delivery date
* procurement units (milliliters, kilograms, pieces, ...)
* procurement amount
* best before date
* status: available for production?
* properties (arbitraty key-value data, stored as JSON string)

Ingredient batches SHOULD be referenced to have:

* an invoice (document)
* a delivery note (document)
* photos (document)
* an inspection report (ingredient batch event)

## Documment

* document type
* name
* upload date
* S3 bucket
* Object ID ("key" in AWS-speak)
* URL

## Ingredient Batch Document

* ingredient batch
* document

## S3 bucket

* name (DNS compliant, no dots)
* description
* upload priority (which bucket to use for uploads at the moment, highest wins)
* base URL (virtual-host-style URL)
* read-only credentials (will be given to authenticated micro-service clients)
* CRUD credentials

## Ingredient Batch Event

This is intended to enable a timeline of events during batch lifetime,
such as for taking temperature or other measurements, goods inspections,
processing steps, etc.

* ingredient batch
* event date
* event type
* event data (arbitrary JSON data, interpretation will depend on event type)

## Ingredient

A material we use in production that ends up as part of the final
product, such as honey or fruits.

We store these pieces of information per ingredient type:

* manufacturer (third party)
* product name
* default package units
* default package amount

## Third Party

* name
* contact name
* address, zip, city, country
* phone
* email
* homepage URL
* our ID with them
* CRM ID

## Production Batches

This is a batch of our final product, or a stage towards a product.
Examples are "cherry mead", "classic mead" or similar.  We track these
so we have a record of what went into them, and where we sold them to.

Note that production batches can depend on each other. For example,
a 100 liter batch might ferment, and then 30 liters are extracted,
while the remaining 70 liter continue fermenting.  In this case,
the 30 liters become a production batch of their own.  Further
ingredients might then be added to either batch, which would require
a new batch number.

Also, mixing production batches would result into a "blend", which
is perfectly fine, but is also a new production batch to keep track of.

We store these pieces of information per production batch:

* lokyr batch name (e.g. "L2018-51")
* product (strawberry mead, raspberry mead, ginger beer, ...)
* batch size (liters)
* status: currently in production?
* bottling date
* best before date
* alcohol content
* retainedSamples
* properties (arbitrary key-value data, stored as JSON string)

Production batches SHOULD be referenced to have:

* laboratory result reports (document)
* fermentation start (event)
* alcohol measurement (event)

## Ingredient Batch Relation

* ingredient batch
* production batch
* date of addition
* units
* amount

## Production Batch Split

* parent production batch
* date of split
* amount (liters)
* child production batch

## Production Batch Blend

* source production batch A
* source production batch B
* date of blend
* destination production batch

## Production Batch Document

* production batch
* document

## Production Event

This is intended to enable a timeline of events during batch production,
such as for taking temperature or alcohol measurements, tastings, inspections,
bottling, filtering steps, rackings, fermentation start, etc.

* production batch
* event date
* event type
* event properties (key-value data, interpretation will depend on event type)

## Event Type

* name
* class (one of "measurement", "production step", "note", "other")

## Shipments

* production batch
* delivery date
* delivery units
* delivery amount
* recipient name
* recipient address
* recipient ID in CRM (future use)
