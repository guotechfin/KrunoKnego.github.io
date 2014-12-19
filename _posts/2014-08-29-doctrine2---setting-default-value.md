---
layout: post
title: "Doctrine2 setting default value"
category: 
date: " 2014-08-29 10:42:21"
tags: []
---


![Doctrine2]({{ site.url }}/assets/doctrine2.png "Doctrine Image"){: .center-image}

There are two ways of setting default values for Doctrine2 entities.

The way I was familiar with is just to define your default value in construct of your entity.

{% highlight php5 %}
    <?php

    namespace Acme\Bundle\AcmeBundle;

    class AcmeEntity
    {

        private $acmeOne;

        function __construct($acmeOne)
        {
            $this->setAcmeOne = $acmeOne;
        }



        /**
         * Set acmeOne
         *
         * @param string $acmeOne
         * @return AcmeEntity
         */
        public function setAcmeOne($acmeOne)
        {
            $this->acmeOne = $acmeOne;

            return $this;
        }

        /**
         * Get acmeOne
         *
         * @return string
         */
        public function getAcmeOne()
        {
            return $this->acmeOne;
        }
    }
{% endhighlight %}

The other way adding default value to your entity is through schema.
Now this differs if you use annotations, xml or php.
I'm going to give you example for xml.

So the same thing you define your AcmeEntity.orm.xml

{% highlight xml+mako %}
    <?xml version="1.0" encoding="utf-8"?>
    <doctrine-mapping xmlns="http://doctrine-project.org/schemas/orm/doctrine-mapping"
                      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                      xsi:schemaLocation="http://doctrine-project.org/schemas/orm/doctrine-mapping http://doctrine-project.org/schemas/orm/doctrine-mapping.xsd">
        <entity name="Acme\Bundle\AcmeBundle\Entity\AcmeEntity" table="acme_entity">
            <id name="id" type="integer" column="id">
                <generator strategy="AUTO"/>
            </id>

            <field name="acmeOne" type="string" column="acmeOne" length="36">
                <options>
                    <option name="comment">Your comment goes here.</option>
                    <option name="default">Default Value</option>
                </options>
            </field>

        </entity>
</doctrine-mapping>
{% endhighlight %}

This just goes to show no matter how many times you go over Doctrine2 documentation there is always
something new to learn.