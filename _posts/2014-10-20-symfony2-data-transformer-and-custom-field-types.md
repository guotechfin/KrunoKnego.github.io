---
layout: post
title: "Symfony2 Data Transformer and Custom Field Types"
category: 
date: " 2014-10-20 09:05:23"
tags: []
---

I was working on a project that dealt with sport analytics and Symfony2 forms were very slow
because I had too much many-to-many and one-to-many entities loaded. Those kinds of problems are easily
fixed using data transformers. You can read more about them <a href="http://symfony.com/doc/current/cookbook/form/data_transformers.html">here</a>.

The reason why I'm writing this blog post is because I've set it up a little bit differently.
All those entities that were causing the problem required the same transformer. The only thing that differed
was the repository. Because of those reasons there was no need to needlessly replicate data transformers and
custom field types.

Thus I've used dependency injection. In data transformer in addition to injecting object manager
I've injected the repository and in the custom field type I injected name of custom field type.

Below you can see an example of that implementation.


This is data transform which was injected with repository and object manager.

{% highlight php5 %}
<?php

namespace AcmeBundle\Form\DataTransformer;

use Doctrine\Common\Persistence\ObjectManager;
use Symfony\Component\Form\DataTransformerInterface;
use Symfony\Component\Form\Exception\TransformationFailedException;

class RelatedTransformer implements DataTransformerInterface
{
    private $om;

    private $repo;

    public function __construct(ObjectManager $om, $repo)
    {
        $this->om = $om;
        $this->repo = $repo;
    }

    public function transform($issue)
    {
        if (null === $issue) {
            return "";
        }

        $result = '';
        foreach ($issue as $post) {
            $result .= $post->getId().',';
        }

        return $result;
    }

    public function reverseTransform($string)
    {
        if (!$string) {
            return new \Doctrine\Common\Collections\ArrayCollection();
        }

        $tempArr = explode(',', $string);

        $res = new \Doctrine\Common\Collections\ArrayCollection();

        $repo = $this->repo;

        foreach ($tempArr as $id) {
            $post = $repo->find($id);
            if (is_object($post)) {
                $res->add($post);
            }
        }

        return $res;
    }

}
{% endhighlight %}


Here you can see a custom filed type. In addition to object manager and repository I had to inject
name of custom field type.

{% highlight php5 %}
<?php

namespace AcmeBundle\Form\Selector;

use Symfony\Component\Form\AbstractType;
use Symfony\Component\Form\FormBuilderInterface;
use AcmeBundle\Form\DataTransformer\RelatedTransformer;
use Doctrine\Common\Persistence\ObjectManager;
use Symfony\Component\OptionsResolver\OptionsResolverInterface;

class RelatedSelectorType extends AbstractType
{
    /**
     * @var ObjectManager
     */
    protected $om;

    protected $repo;

    protected $name;

    public function __construct($name, ObjectManager $om, $repo)
    {
        $this->name = $name;
        $this->om = $om;
        $this->repo = $repo;
    }

    public function buildForm(FormBuilderInterface $builder, array $options)
    {
        $transformer = new RelatedTransformer($this->om, $this->repo);
        $builder->addModelTransformer($transformer);
    }

    public function setDefaultOptions(OptionsResolverInterface $resolver)
    {
        $resolver->setDefaults(array(
            'invalid_message' => 'The selected sport does not exist',
        ));
    }

    public function getParent()
    {
        return 'hidden';
    }

    public function getName()
    {
        return $this->name;
    }
}

{% endhighlight %}


This is my services.xml file. I prefer using xml for definining my services because when I've used
yml format too often I had problem with wrong indentation.

{% highlight php5 %}
<?xml version="1.0" ?>

<container xmlns="http://symfony.com/schema/dic/services"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://symfony.com/schema/dic/services http://symfony.com/schema/dic/services/services-1.0.xsd">


    <parameters>
        <parameter key="acme.repository.post.class">AcmeBundle\Repository\PostRepository</parameter>
        <parameter key="acme.model.post.class">AcmeBundle\Entity\Post</parameter>
    </parameters>

    <services>
        <service id="acme.form.type.post" class="AcmeBundle\Form\Type\PostType">
            <tag name="form.type" alias="acme_post" />
        </service>

        <service id="acme.form.type.category" class="AcmeBundle\Form\Type\CategoryType">
            <tag name="form.type" alias="acme_category" />
        </service>

        <service id="acme.manager.post" alias="doctrine.orm.entity_manager" />
        <service id="acme.repository.post" class="%acme.repository.post.class%">
            <argument type="service" id="acme.manager.post" />
            <argument type="service">
                <service
                        factory-service="doctrine.orm.default_entity_manager"
                        factory-method="getClassMetadata"
                        class="Doctrine\ORM\Mapping\ClassMetadata"
                        >
                    <argument>%acme.model.post.class%</argument>
                </service>
            </argument>
        </service>

        <service class="AcmeBundle\Form\Selector\RelatedSelectorType" id="acme_bundle.form.selector.relatedselectortype">
            <argument type="string">related_posts_selector</argument>
            <argument id="doctrine.orm.default_entity_manager" type="service"/>
            <argument id="acme.repository.post" type="service"/>
            <tag alias="related_posts_selector" name="form.type"/>
        </service>

        <!-- PEOPLE -->
        <service class="AcmeBundle\Form\Selector\RelatedSelectorType" id="acme_bundle1.form.selector.relatedselectortype">
            <argument type="string">related_soccer_people_selector</argument>
            <argument id="doctrine.orm.default_entity_manager" type="service"/>
            <argument id="acme_soccer.repository.people" type="service"/>
            <tag alias="related_soccer_people_selector" name="form.type"/>
        </service>

        <service class="AcmeBundle\Form\Selector\RelatedSelectorType" id="acme_bundle2.form.selector.relatedselectortype">
            <argument type="string">related_hockey_people_selector</argument>
            <argument id="doctrine.orm.default_entity_manager" type="service"/>
            <argument id="acme_hockey.repository.people" type="service"/>
            <tag alias="related_hockey_people_selector" name="form.type"/>
        </service>

        <service class="AcmeBundle\Form\Selector\RelatedSelectorType" id="acme_bundle3.form.selector.relatedselectortype">
            <argument type="string">related_handball_people_selector</argument>
            <argument id="doctrine.orm.default_entity_manager" type="service"/>
            <argument id="acme_handball.repository.people" type="service"/>
            <tag alias="related_handball_people_selector" name="form.type"/>
        </service>

        <service class="AcmeBundle\Form\Selector\RelatedSelectorType" id="acme_bundle4.form.selector.relatedselectortype">
            <argument type="string">related_basketball_people_selector</argument>
            <argument id="doctrine.orm.default_entity_manager" type="service"/>
            <argument id="acme_basketball.repository.people" type="service"/>
            <tag alias="related_basketball_people_selector" name="form.type"/>
        </service>

        <!-- TEAMS -->
        <service class="AcmeBundle\Form\Selector\RelatedSelectorType" id="acme_bundle11.form.selector.relatedselectortype">
            <argument type="string">related_soccer_teams_selector</argument>
            <argument id="doctrine.orm.default_entity_manager" type="service"/>
            <argument id="acme_soccer.repository.team" type="service"/>
            <tag alias="related_soccer_teams_selector" name="form.type"/>
        </service>

        <service class="AcmeBundle\Form\Selector\RelatedSelectorType" id="acme_bundle22.form.selector.relatedselectortype">
            <argument type="string">related_hockey_teams_selector</argument>
            <argument id="doctrine.orm.default_entity_manager" type="service"/>
            <argument id="acme_hockey.repository.team" type="service"/>
            <tag alias="related_hockey_teams_selector" name="form.type"/>
        </service>

        <service class="AcmeBundle\Form\Selector\RelatedSelectorType" id="acme_bundle33.form.selector.relatedselectortype">
            <argument type="string">related_handball_teams_selector</argument>
            <argument id="doctrine.orm.default_entity_manager" type="service"/>
            <argument id="acme_handball.repository.team" type="service"/>
            <tag alias="related_handball_teams_selector" name="form.type"/>
        </service>

        <service class="AcmeBundle\Form\Selector\RelatedSelectorType" id="acme_bundle44.form.selector.relatedselectortype">
            <argument type="string">related_basketball_teams_selector</argument>
            <argument id="doctrine.orm.default_entity_manager" type="service"/>
            <argument id="acme_basketball.repository.team" type="service"/>
            <tag alias="related_basketball_teams_selector" name="form.type"/>
        </service>

        <!-- SEASONS -->
        <service class="AcmeBundle\Form\Selector\RelatedSelectorType" id="acme_bundle111.form.selector.relatedselectortype">
            <argument type="string">related_soccer_seasons_selector</argument>
            <argument id="doctrine.orm.default_entity_manager" type="service"/>
            <argument id="acme_soccer.repository.season" type="service"/>
            <tag alias="related_soccer_seasons_selector" name="form.type"/>
        </service>

        <service class="AcmeBundle\Form\Selector\RelatedSelectorType" id="acme_bundle222.form.selector.relatedselectortype">
            <argument type="string">related_hockey_seasons_selector</argument>
            <argument id="doctrine.orm.default_entity_manager" type="service"/>
            <argument id="acme_hockey.repository.season" type="service"/>
            <tag alias="related_hockey_seasons_selector" name="form.type"/>
        </service>

        <service class="AcmeBundle\Form\Selector\RelatedSelectorType" id="acme_bundle333.form.selector.relatedselectortype">
            <argument type="string">related_handball_seasons_selector</argument>
            <argument id="doctrine.orm.default_entity_manager" type="service"/>
            <argument id="acme_handball.repository.season" type="service"/>
            <tag alias="related_handball_seasons_selector" name="form.type"/>
        </service>

        <service class="AcmeBundle\Form\Selector\RelatedSelectorType" id="acme_bundle444.form.selector.relatedselectortype">
            <argument type="string">related_basketball_seasons_selector</argument>
            <argument id="doctrine.orm.default_entity_manager" type="service"/>
            <argument id="acme_basketball.repository.season" type="service"/>
            <tag alias="related_basketball_seasons_selector" name="form.type"/>
        </service>
    </services>

</container>


{% endhighlight %}


Finally after all that configuration you can use your custom field types.

{% highlight php5 %}
<?php

namespace AcmeBundle\Form\Type;

use Symfony\Component\Form\AbstractType;
use Symfony\Component\Form\FormBuilderInterface;
use Symfony\Component\OptionsResolver\OptionsResolverInterface;

class PostType extends AbstractType
{
    public function buildForm(FormBuilderInterface $builder, array $options)
    {
        $builder
            ->add('relatedSoccerSeasons', 'related_soccer_seasons_selector', array(
                    'required' => false,
                ))
            ->add('relatedSoccerTeams', 'related_soccer_teams_selector', array(
                    'required' => false,
                ))
            ->add('relatedSoccerPeople', 'related_soccer_people_selector', array(
                    'required' => false,
                ))
            ->add('relatedHockeySeasons', 'related_hockey_seasons_selector', array(
                'required' => false,
            ))
            ->add('relatedHockeyTeams', 'related_hockey_teams_selector', array(
                'required' => false,
            ))
            ->add('relatedHockeyPeople', 'related_hockey_people_selector', array(
                'required' => false,
            ))
            ->add('relatedBasketballSeasons', 'related_basketball_seasons_selector', array(
                'required' => false,
            ))
            ->add('relatedBasketballTeams', 'related_basketball_teams_selector', array(
                'required' => false,
            ))
            ->add('relatedBasketballPeople', 'related_basketball_people_selector', array(
                'required' => false,
            ))
            ->add('relatedHandballSeasons', 'related_handball_seasons_selector', array(
                'required' => false,
            ))
            ->add('relatedHandballTeams', 'related_handball_teams_selector', array(
                'required' => false,
            ))
            ->add('relatedHandballPeople', 'related_handball_people_selector', array(
                'required' => false,
            ))

            ...
        ;
    }
}

{% endhighlight %}

<b>EDIT 01.11.2014</b>

{% highlight php5 %}
<?php

namespace AcmeBunde\Controller;

use Symfony\Bundle\FrameworkBundle\Controller\Controller;
use Symfony\Component\HttpFoundation\JsonResponse;
use Symfony\Component\HttpFoundation\Request;
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\HttpKernel\Exception\BadRequestHttpException;

class Select2Controller extends Controller
{
    public function relatedPostAction(Request $request)
    {
        if ($request->isXmlHttpRequest()) {

            // retrieve query and page_limit
            $q = $request->query->get('term');
            $p = $request->query->get('page_limit');

            $posts = $this
                ->get('acme_blog.repository.post')
                ->createQueryBuilder('p')
                ->select('p.id, p.title AS term')
                ->where('p.title LIKE :q')
                ->setParameter('q', '%'.$q.'%')
                ->setMaxResults($p)
                ->getQuery()
                ->getResult()
            ;

            $arr = array('results' => array('posts' => $posts));

            $response = new JsonResponse($arr);
            $response->setCallback($request->query->get('callback'));

            return $response;
        }

        throw new BadRequestHttpException('Only XHR supported');
    }

    public function relatedPeopleAction($sport, Request $request)
    {
        if ($request->isXmlHttpRequest()) {

            // retrieve query and page_limit
            $q = $request->query->get('term');
            $p = $request->query->get('page_limit');

            $posts = $this
                ->get('acme_'.$sport.'.repository.people')
                ->createQueryBuilder('p')
                ->select('p.id, p.name AS term')
                ->where('p.name LIKE :q')
                ->setParameter('q', '%'.$q.'%')
                ->setMaxResults($p)
                ->getQuery()
                ->getResult()
            ;

            $arr = array('results' => array('posts' => $posts));

            $response = new JsonResponse($arr);
            $response->setCallback($request->query->get('callback'));

            return $response;
        }

        throw new BadRequestHttpException('Only XHR supported');
    }

    public function relatedTeamsAction($sport, Request $request)
    {
        if ($request->isXmlHttpRequest()) {

            // retrieve query and page_limit
            $q = $request->query->get('term');
            $p = $request->query->get('page_limit');

            $posts = $this
                ->get('acme_'.$sport.'.repository.team')
                ->createQueryBuilder('p')
                ->select('p.id, p.clubName AS term')
                ->where('p.clubName LIKE :q')
                ->setParameter('q', '%'.$q.'%')
                ->setMaxResults($p)
                ->getQuery()
                ->getResult()
            ;

            $arr = array('results' => array('posts' => $posts));

            $response = new JsonResponse($arr);
            $response->setCallback($request->query->get('callback'));

            return $response;
        }

        throw new BadRequestHttpException('Only XHR supported');
    }

    public function relatedSeasonsAction($sport, Request $request)
    {
        if ($request->isXmlHttpRequest()) {

            // retrieve query and page_limit
            $q = $request->query->get('term');
            $p = $request->query->get('page_limit');

            $posts = $this
                ->get('acme_'.$sport.'.repository.season')
                ->createQueryBuilder('p')
                ->select('p.id, p.name AS term')
                ->where('p.name LIKE :q')
                ->setParameter('q', '%'.$q.'%')
                ->setMaxResults($p)
                ->getQuery()
                ->getResult()
            ;

            $arr = array('results' => array('posts' => $posts));

            $response = new JsonResponse($arr);
            $response->setCallback($request->query->get('callback'));

            return $response;
        }

        throw new BadRequestHttpException('Only XHR supported');
    }
}
{% endhighlight %}


{% highlight php5 %}
$(document).ready(function() {
    function select2Helper(element, path) {
        element.select2({
            placeholder: "Search ",
            multiple: true,
            minimumInputLength: 1,
            ajax: {
                url: path,
                dataType: 'jsonp',
                data: function (term, page) {
                    return {
                        term: term, // search term
                        page_limit: 10
                    };
                },
                results: function (data) {
                    return {results: data.results.posts};
                }
            },
            formatResult: function(posts) {
                return "<div class='select2-user-result'>" + posts.term + "</div>";
            },
            formatSelection: function(posts) {
                return posts.term;
            },
            initSelection : function (element, callback) {
                var elementText = $(element).attr('data-init-text');
                callback({"term": elementText});
            }
        });
    }

    select2Helper($("#acme_blog_post_relatedPosts"), "{{ url('acme_related_posts_select2') }}");

    select2Helper($("#acme_blog_post_relatedSoccerPeople"), "{{ url('acme_related_people_select2', {'sport':'soccer'}) }}");
    select2Helper($("#acme_blog_post_relatedBasketballPeople"), "{{ url('acme_related_people_select2', {'sport':'basketball'}) }}");
    select2Helper($("#acme_blog_post_relatedHandballPeople"), "{{ url('acme_related_people_select2', {'sport':'handball'}) }}");
    select2Helper($("#acme_blog_post_relatedHockeyPeople"), "{{ url('acme_related_people_select2', {'sport':'hockey'}) }}");

    select2Helper($("#acme_blog_post_relatedSoccerTeams"), "{{ url('acme_related_teams_select2', {'sport':'soccer'}) }}");
    select2Helper($("#acme_blog_post_relatedBasketballTeams"), "{{ url('acme_related_teams_select2', {'sport':'basketball'}) }}");
    select2Helper($("#acme_blog_post_relatedHandballTeams"), "{{ url('acme_related_teams_select2', {'sport':'handball'}) }}");
    select2Helper($("#acme_blog_post_relatedHockeyTeams"), "{{ url('acme_related_teams_select2', {'sport':'hockey'}) }}");

    select2Helper($("#acme_blog_post_relatedSoccerSeasons"), "{{ url('acme_related_seasons_select2', {'sport':'soccer'}) }}");
    select2Helper($("#acme_blog_post_relatedBasketballSeasons"), "{{ url('acme_related_seasons_select2', {'sport':'basketball'}) }}");
    select2Helper($("#acme_blog_post_relatedHandballSeasons"), "{{ url('acme_related_seasons_select2', {'sport':'handball'}) }}");
    select2Helper($("#acme_blog_post_relatedHockeySeasons"), "{{ url('acme_related_seasons_select2', {'sport':'hockey'}) }}");

});
{% endhighlight %}



