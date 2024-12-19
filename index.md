---
layout: page
css: /assets/css/index.css
full-width: true
---

<div id="main-cover">
  <div id="cover-shield"></div>
  <div id="cover-unshield">
    <div class="main-title">Software</div>
    <div class="main-title">Engineering</div>
    <div class="main-title">Services</div>
    <div class="main-subtitle">SERVING THE PACIFIC NORTHWEST SINCE 1996</div>
  </div>
</div>

<div id="process">
  <div class="container-xl">
    <div class="row">
      <div class="col-lg-6">
        <h2>IT’S EASY TO WORK WITH US:</h2>
<!--        <a class="process-title" href="{{ '/automation' | relative_url }}">1. Review our process automation offerings&nbsp;&nearr;</a>  -->
        <a class="process-title" href="{{ '/services' | relative_url }}">1. Review our services&nbsp;&nearr;</a>
        <div>Review our service offerings and see if we may be a good match for your needs.</div>
        <a class="process-title" href="{{ '/about-us' | relative_url }}">2. Check out our expertise&nbsp;&nearr;</a>
        <div>Get a feel for the kind of projects we can deliver.</div>
        <a class="process-title" href="{{ '/contact' | relative_url }}">3. Schedule a discussion&nbsp;&nearr;</a>
        <div>Enjoy a relaxed discussion of your project and your requirements. ...</div>
      </div>
      <div class="col-lg-6">
        <div class="center">
          <iframe width="560" height="315" src="https://www.youtube.com/embed/maSB8s3YYEg" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
        </div>
        <div id="video-caption">Dr. Atul Gawande on the importance of finding a coach.</div>
      </div>
    </div>
  </div>
</div>

<div id="home-entries" class="container-xl">
  <div class="row">
    <div class="col-xl-8 offset-xl-2 col-lg-10 offset-lg-1">
      <h2 class="space-top"><a href="{{ '/blog' | relative_url }}">Our Blog</a> – latest entries:</h2>
      <div class="all-entries">
        {% for post in site.posts limit:3 %}
        <div class="entry">
          {% if post.thumbnail-img %}
            <div class="entry-image rounded">
              <a href="{{ post.url | absolute_url }}" aria-label="Thumbnail">
                <img src="{{ post.thumbnail-img | absolute_url }}" alt="{{ post.title | strip_html }}">
              </a>
            </div>
          {% endif %}
          <a class="entry-title" href="{{ post.url | absolute_url }}">{{ post.title | strip_html }}</a>
          {% if post.author %}
          <div class="entry-author">By {{ post.author }}</div>
          {% endif %}
          <div class="entry-excerpt">{{ post.excerpt | strip_html | truncatewords: 55 }}</div>
          <a href="{{ post.url | absolute_url }}" class="entry-read-more secondarybtn">Read&nbsp;More</a>
        </div>
        {% endfor %}
      </div>
    </div>
  </div>
</div>
