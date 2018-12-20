---
title: ECTS Extension
subtitle: Community-developed addition to the Open Badges Specification
show_sidebar: true
layout: page
---

### <a id="ECTS"></a>ECTS Extension
_Author: [Frans Ward, Ronald Ham](http://edubadges.nl)_

European Credit Transfer and Accumulation System (ECTS) credits are a standard means for comparing the "volume of learning based on the defined learning outcomes and their associated workload" for higher education across the European Union and other collaborating European countries. For successfully completed studies, ECTS credits are awarded. One academic year corresponds to 60 ECTS credits that are normally equivalent to 1500–1800 hours of total workload, irrespective of standard or qualification type. ECTS credits are used to facilitate transfer and progression throughout the European Union. 

The ECTS value of a Badge Class serves to publicly acknowledge the ECTS value of a badge as *designed, assessed, and issued by a badge issuer*.  See the [context](./ectsextension/context.json) and [schema](./ectsextension/schema.json) for details.

Property     | Type        | Value Description
-------------|-------------|---------
**@context** | context IRI | [https://w3id.org/openbadges/extensions/ectsExtension/context.json](context.json)
**type**    | type IRI array |`['Extension', 'extensions:ectsExtension']`
**description** | string | The endorser's statement about the endorsement of this particular badge class.
endorsedObject | object | An optional embedded copy of the endorsed Badge Object with 'id' attribute set.

**Extendable Badge Objects:**
Assertion, BadgeClass, Issuer

This extension is a little more involved in that it requires a whole Badge Class to be created for each issuer who wants to use it. From there, the extension itself must be added to an Assertion whose recipient identity is the IRI (URL) of the endorsed Badge Object. Endorsers may use one Badge Class for all their endorsements, or they may create multiple badge classes to provide different types of endorsement. Consumers should consider both the Badge Class's `description` field as well as the Assertion's Endorsement extension's `description` property. The extension poses the option to embed the entire endorsed object so questions of intent could be resolved in case the hosted endorsed object is changed by its issuer.

**Example implementation:**

BadgeClass (each endorsing issuer must define a BadgeClass to use for endorsement):
{% highlight json %}
{
  "@context": "https://w3id.org/openbadges/v1",
  "id": "http://endorser.org/endorsementclass1",
  "type": "BadgeClass",
  "name": "Endorsement Badge",
  "description": "Endorser's badge of support. Badges receiving this have been vetted by the Endorser.",
  "image": "http://endorser.org/endorsementimage.png",
  "criteria": "http://endorser.org/what-it-takes",
  "issuer": "http://endorser.org/issuer1"
}
{% endhighlight %}

Assertion (full copy of endorsed object elided):
{% highlight json %}
{ 
  "@context": "https://w3id.org/openbadges/v1",
  "id": "http://endorser.org/endorsement124",
  "type": "Assertion",
  "recipient": {
    "identity": "http://anotherissuer.org/badgeclass5",
    "type": "@id",
    "hashed": false 
  },
  "badge": "http://endorser.org/endorsementclass1",
  "extensions:Endorsement": {
    "@context":"https://w3id.org/openbadges/extensions/endorsement/context.json",
    "type": ["Extension", "extensions:Endorsement"],
    "description": "This badge is truly a work of art, and meaningful for its earners besides.",
    "endorsedObject": {
        "**": "*** Full copy of endorsed object ***"
    }
  },
  "issuedOn": "2015-05-20"
}
{% endhighlight %}