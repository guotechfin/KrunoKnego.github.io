---
layout: post
title: "Simple regex twig extension"
category: 
date: " 2014-08-28 17:55:46"
tags: [symfony2, regex, twig]
---

![Twig]({{ site.url }}/assets/twig.jpg ){: .center-image }

This will be a short tutorial how to build twig extension that uses regular expressions.
Let's say you have a html document in your variable and you want to extract image or paragraph
from that variable. If you were to look at twig's filters and functions you'd soon notice there's nothing
that can help you accomplish that task  <a title="TWIG Documentation" href="http://twig.sensiolabs.org/documentation" target="_blank">Twig Documentation</a>

Luckily you can easily extend twig's initial set of functions and filters and it's fairly simple.

First create folder named Twig in your bundle and then create RegexExtension.php and copy paste the following.

{% highlight php5 %}
    <?php

    namespace Acme\Bundle\AcmeBundle\Twig;

    class RegexExtension extends \Twig_Extension
    {
        public function getFilters()
        {
            return array(
                'regex' => new \Twig_Filter_Method($this, 'regex'),
                'regexPhoto' => new \Twig_Filter_Method($this, 'regexPhoto'),
            );
        }

        public function regex($body)
        {
            // extract one image
            preg_match('/\<img.+\\/\>/', $body, $img);

            // extract one paragraph
            preg_match('/\<p\>.+\<\\/p\>/', $body, $p);

            return $p[0].'</br>'.$img[0];
        }

        public function regexPhoto($body)
        {
            // extract one image
            preg_match('/\<img.+\\/\>/', $body, $img);

            return $img[0];
        }


        public function getName()
        {
            return 'regex_extension';
        }

    }
{% endhighlight %}

Don't forget to change namespace to match that of your application.

Check that your file structure matches:

AcmeBundle
    -- Resources
        -- config
                services.xml


Copy paste the following to enable your twig extension.

{% highlight xml+mako %}
    <container xmlns="http://symfony.com/schema/dic/services"
               xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
               xsi:schemaLocation="http://symfony.com/schema/dic/services http://symfony.com/schema/dic/services/services-1.0.xsd">

        <services>

            <service class="Acme\Bundle\AcmeBundle\Twig\RegexExtenstion" id="acme_bundle.twig.regexextenstion">
                <call method="initRuntime">
                    <argument id="twig" type="service"/>
                </call>
                <tag name="twig.extension"/>
            </service>


        </services>
    </container>
{% endhighlight %}

Also don't forget to make your dependency injection folder and required files.

Your file structure should match this one:

AcmeBundle
    -- DependencyInjection
        AcmeExtension.php
        Configuration.php

Your configuration file should look like this:

{% highlight php5 %}
    <?php

    namespace Acme\Bundle\AcmeBundle\DependencyInjection;

    use Symfony\Component\Config\Definition\Builder\TreeBuilder;
    use Symfony\Component\Config\Definition\ConfigurationInterface;

    /**
     * This is the class that validates and merges configuration from your app/config files
     *
     * To learn more see {@link http://symfony.com/doc/current/cookbook/bundles/extension.html#cookbook-bundles-extension-config-class}
     */
    class Configuration implements ConfigurationInterface
    {
        /**
         * {@inheritDoc}
         */
        public function getConfigTreeBuilder()
        {
            $treeBuilder = new TreeBuilder();
            $rootNode = $treeBuilder->root('acme');

            // Here you should define the parameters that are allowed to
            // configure your bundle. See the documentation linked above for
            // more information on that topic.

            return $treeBuilder;
        }
    }
{% endhighlight %}

And your extension file like this:

{% highlight php5 %}
    <?php

    namespace Acme\Bundle\AcmeBundle\DependencyInjection;

    use Symfony\Component\DependencyInjection\ContainerBuilder;
    use Symfony\Component\Config\FileLocator;
    use Symfony\Component\HttpKernel\DependencyInjection\Extension;
    use Symfony\Component\DependencyInjection\Loader;

    /**
     * This is the class that loads and manages your bundle configuration
     *
     * To learn more see {@link http://symfony.com/doc/current/cookbook/bundles/extension.html}
     */
    class AcmeExtension extends Extension
    {
        /**
         * {@inheritDoc}
         */
        public function load(array $configs, ContainerBuilder $container)
        {
            $configuration = new Configuration();
            $config = $this->processConfiguration($configuration, $configs);

            $loader = new Loader\XmlFileLoader($container, new FileLocator(__DIR__.'/../Resources/config'));
            $loader->load('services.xml');
        }
    }
{% endhighlight %}


Now to use your newly created filter within twig template you just write:

{% raw %}
    {{ varWithHtml|regex|raw }}
{% endraw %}

That will extract one image and one paragraph.

{% raw %}
    {{ varWithHtml|regexPhoto|raw }}
{% endraw %}

This will extract only one image.
Raw filter is used to show the image and paragraph.

If you would like to create twig functions you just change getFilters method to getFunctions.
This is only small part of what can twig do for you and if you're using PHP and looking for
a templating engine I would definitely recommend you to start using twig.

Before you start writing your own extensions first check twigs filters and functions because
oftentimes there is some hidden feature you might have missed.
