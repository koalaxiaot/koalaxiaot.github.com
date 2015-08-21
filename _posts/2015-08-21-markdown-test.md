---
layout: post
---

- [ ] a bigger project
  - [ ] first subtask #1234
  - [ ] follow up subtask #4321
- [x] final subtask cc @mention
- [ ] a separate task

- [x] @mentions, #refs, [links](), **formatting**, and <del>tags</del> are supported 
- [x] list syntax is required (any unordered or ordered list supported) 
- [x] this is a complete item 
- [ ] this is an incomplete item

1. [ ] asdfjalsdjfalsdf asdlfjasldf 
2. [ ] asdfjalsdjfalsdf asdlfjasldf 

{% highlight ruby linenos %}
	def show
    @widget = Widget(params[:id])
	  respond_to do |format|
	    format.html # show.html.erb
	    format.json { render json: @widget }
	  end
	end
{% endhighlight %}

{% highlight ruby %}
	def show
    @widget = Widget(params[:id])
	  respond_to do |format|
	    format.html # show.html.erb
	    format.json { render json: @widget }
	  end
	end
{% endhighlight %}
