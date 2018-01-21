---
layout: post
title: How to build a status page with Jekyll and Uptimerobot
header: How to build a Jekyll status page
category: Features
image: /images/blog/status-page/banner@2x.png
author: george
---

Public visibility of an apps performance is vital to the trust placed on that product. Today we announce our [new status page](http://status.cloudcannon.com/) and [twitter account](https://twitter.com/CCAppStatus) focused on greater visibility into our performance and uptime. Our status page was built using Jekyll and [Uptimerobot](https://uptimerobot.com/). This allows us to host the site anywhere with minimal cost. Building this site in Jekyll gives the added benefit of being fully customisable with any HTML, CSS or JavaScript.

[![The banner of the CloudCannon status page](/images/blog/status-page/banner.png){: .screenshot srcset="/images/blog/status-page/banner.png 800w, /images/blog/status-page/banner@2x.png 1600w"}](http://status.cloudcannon.com/)

This article will break down how each section was created and their purpose. An unbranded version of the status page is available on GitHub.

### Components

The components section describes the different parts of your application. CloudCannon consists of six different components which we can toggle into different states given an outage or reduced performance. To develop this section we used a Jekyll collection called `components`. The `components` are a set of Markdown files under the `_components` folder which look like:

{% highlight html %}
---
name: File Servers
description: Hosting of CloudCannon websites
state: operational
---
{% endhighlight %}

To output the components using a for loop:

{% highlight html %}
{% raw %}
<ul class="components">
  {% for component in site.components %}
    <li>
      <strong>{{ component.name }}</strong>
      <span class="description">{{ component.description }}</span>
      <span class="badge {{ component.state | slugify }}">{{ component.state }}</span>
    </li>
  {% endfor %}
</ul>
{% endraw %}
{% endhighlight %}

Additionally, we want the title to change if any component state is not set to `operational`. To do this, we loop over the each component again:

{% highlight html %}
{% raw %}
{% assign working = true %}
{% for component in site.components %}
  {% if component.state != "operational" %}
    {% assign working = false %}
  {% endif %}
{% endfor %}

{% if working %}
  <h2 class="operational-heading">All Systems Operational</h2>
{% else %}
  <h2 class="issues-heading">Experiencing Issues</h2>
{% endif %}
{% endraw %}
{% endhighlight %}

This completes the components section where we can easily change the state of all the different parts.

![Displaying the components](/images/blog/status-page/components.png){: .screenshot srcset="/images/blog/status-page/components.png 800w, /images/blog/status-page/components@2x.png 1600w"}

### Incidents

Incidents allow us to provide greater visibility and progress when resolving problems. This is a perfect use case for Jekyll posts. Posts are written in Markdown with an attached date. To read more about writing posts see our [documentation on blogging](https://docs.cloudcannon.com/editing/blogging/).

Incidents are made up of two sections, full history and recent incidents. The full history displays every post grouped by the month that it occurred. The following code uses a for loop to output each post. If the month changes between posts, a header will be output for the month and year.

{% highlight html %}
{% raw %}
{% for post in site.posts %}
  {% capture month %}{{ post.date | date: "%B" }}{% endcapture %}

  {% if month != prev_month %}
    {% unless forloop.first %}<hr>{% endunless %}
    <h2>{{ post.date | date: "%B %Y" }}</h5>
  {% endif %}

  <h3>{{ post.name }} <small>{{ post.date | date: "%A %e" }}</small></h3>

  {{ post.content }}
  {% capture prev_month %}{{ post.date | date: "%B" }}{% endcapture %}
{% endfor %}
{% endraw %}
{% endhighlight %}

Recent incidents on the main page are slightly more complicated:

* We need to output every day in the last ten days
* If a day has no incidents we want to show that positively
* To simplify things we don't want to recompile the site everyday to get the latest day showing 'no incidents'


The best choice for this was JavaScript. Using Liquid we can inject JavaScript into the page to define our incidents:

{% highlight html %}
{% raw %}
<div class="incidents"></div>
<script type="text/javascript" src="//ajax.googleapis.com/ajax/libs/jquery/1.12.0/jquery.min.js"></script>
<script type="text/javascript" src="//cdnjs.cloudflare.com/ajax/libs/moment.js/2.11.1/moment.min.js"></script>
<script type="text/javascript">
window.incidents = [
{% for post in site.posts | sort: "date" %}
  {
    date: new moment({{ post.date | jsonify }}),
    name: {{ post.name | jsonify }},
    content: {{ post.content | jsonify }}
  }{% unless forloop.last %},{% endunless %}
{% endfor %}
];
</script>
{% endraw %}
{% endhighlight %}

Next we need to output an element for each day in the past 10 days, if there are any incidents we will output them. If there are no posts on a day we will output 'No incidents reported'. To help with date formatting and manipulation we are using [moment.js](http://momentjs.com/).

{% highlight javascript %}
(function (window, $, moment) {
  var nextIncidentIndex = 0;

  function formatIncident(date) {
    var html = "<div class='incident-day'><h3>" + date.format("Do MMMM YYYY") + "</h3>",
        incident = window.incidents[nextIncidentIndex];

    if (incident && incident.date.isSame(date, "day")) {
      while(incident.date.isSame(date, "day")) {
        html += "<div class='incident'><h3>" + incident.name + "</h3>" + incident.content + "</div>";
        incident = window.incidents[++nextIncidentIndex];
      }
    } else {
      html += "<p class='no-incidents'>No incidents reported.</p>";
    }

    return html + "</div>";
  }

  var date = moment(),
      end = moment().subtract(10, "days"),
      $incidents = $(".incidents");

  while (date > end) {
    $incidents.append(formatIncident(date));
    date = date.subtract(1, "day");
  }
})(window, window.jQuery, window.moment);
{% endhighlight %}

Adding/updating a post in Git or on CloudCannon will add it to our status page. Alternatively, recent incidents could be implemented using a Jekyll plugin and a daily compile.

![Displaying the incidents](/images/blog/status-page/incidents.png){: .screenshot srcset="/images/blog/status-page/incidents.png 800w, /images/blog/status-page/incidents@2x.png 1600w"}

### Metrics Graphs with Uptimerobot

Response time graphs show the performance and reliability of a system. CloudCannon uses Uptimerobot internally for monitoring. For every service we want to track, we create a monitor on Uptimerobot which pings the service on a regular interval.

Uptimerobot provides a JavaScript API for obtaining response time and uptime data. For each monitor we need to obtain its read key. ​Avoid using a global key, as it provides full control of your account. We opted to create separate monitors for our status page as their API exposes data about our external integrations.

#### Managing monitors with collections

In order to maintain all of the Uptimerobot monitors we used another Jekyll collection called `monitors`. The following is an example monitor for our App:

{% highlight html %}
---
name: CloudCannon App
monitor_id: 777351721
key: "m777351721-73584e57aa39411cdc76df5b"
max: 1500
threshold: 1000
---
{% endhighlight %}

Using the same technique as we used on incidents, we output the monitors to JavaScript:

{% highlight html %}
{% raw %}
<script type="text/javascript">
window.monitors = [
{% for monitor in site.monitors %}
  {
    id: {{ monitor.name | slugify | jsonify }},
    name: {{ monitor.name | jsonify }},
    monitorID: {{ monitor.monitor_id | jsonify }},
    key: {{ monitor.key | jsonify }},
    threshold: {{ monitor.threshold | jsonify }},
    max: {{ monitor.max | jsonify }}
  }{% unless forloop.last %},{% endunless %}
{% endfor %}
];
</script>
{% endraw %}
{% endhighlight %}

#### Requesting the data from Uptimerobot

Now that we have the monitors in `window.monitors`, we need to obtain the data per monitor. For this we created a `getData` function which requests the data and returns an error or the JSON data.

{% highlight javascript %}
function getData(monitor, callback) {
  var now = window.moment(),
    end = now.format("YYYY-MM-DD"),
    startDate = now.subtract(12, "hours"),
    start = startDate.format("YYYY-MM-DD"),
    request = new XMLHttpRequest();

  request.open("GET", "https://api.uptimerobot.com/getMonitors?apiKey=" + monitor.key +
    "&monitors=" + monitor.monitorId +
    "&logs=1" +
    "&customUptimeRatio=1-7-30" +
    "&responseTimes=1" +
    "&responseTimesStartDate=" + start +
    "&responseTimesEndDate=" + end +
    "&format=json&noJsonCallback=1", true);

  request.onload = function () {
    if (request.status >= 200 && request.status < 400) {
      try {
        var json = JSON.parse(request.responseText);
        callback(null, json.monitors.monitor[0]);
      } catch (err) {
        callback(err);
      }
    } else {
      callback(new Error("status:" + request.status));
    }
  };

  request.onerror = function (err) {
    callback(err);
  };

  request.send();
}
{% endhighlight %}

The getData function provides the data we need from Uptimerobot. This data includes the response times in the last twelve hours and the `customUptimeRatio` values for the last 1, 7 and 30 days. To fetch all the data for each monitor we can iterate over them:

{% highlight javascript %}
function addMonitor(monitor) {
  getData(monitor, function (err, data) {
    if (err) {
      console.log(err);
      return;
    }

    // Use the data here
  });
}

// Monitors obtained from jekyll collection
for (var i = 0; i < window.monitors.length; i++) {
  addMonitor(window.monitors[i]);
}
{% endhighlight %}

#### Displaying the metrics

To display the data we used a combination of Jekyll and JavaScript. On our index page we output an outline for the graphs with Jekyll. This prevents a shift in window height when the data loads and gives an indeterminate state to the viewers.

{% highlight html %}
{% raw %}
<h2>System Metrics</h2>
{% for monitor in site.monitors %}
<div class="metric" id="{{ monitor.name | slugify }}">
	<h3>{{ monitor.name }}</h3>
	<p class="response-time">Fetching...</p>
	<div class="graph loading"></div>
	<ul class="uptimes">
		<li class="uptime uptime-24-hour"><span>...</span> <strong>Uptime in the last<br>24 Hours</strong></li>
		<li class="uptime uptime-7-days"><span>...</span> <strong>Uptime in the last<br>7 Days</strong></li>
		<li class="uptime uptime-30-days"><span>...</span> <strong>Uptime in the last<br>30 Days</strong></li>
	</ul>
</div>
{% endfor %}
{% endraw %}
{% endhighlight %}

The uptime values are the easiest to begin with:

{% highlight javascript %}
...
var $container = $("#" + monitor.id);
getData(monitor, function (err, data) {
  if (err) {
    console.log(err);
    return;
  }

  var custom = data.customuptimeratio.split("-");
  $container.find(".uptime-24-hour span").html(custom[0] + "<small>%</small>");
  $container.find(".uptime-7-days span").html(custom[1] + "<small>%</small>");
  $container.find(".uptime-30-days span").html(custom[2] + "<small>%</small>");
});
...
{% endhighlight %}

Before we can show the data from Uptimerobot we need to process it. We want to make an average of all of the data for the `.response-time` element and we need to change it into a form our graphing library will understand.

{% highlight javascript %}
...
var $container = $("#" + monitor.id);
getData(monitor, function (err, data) {
  ...
  var points = [],
      sum = 0;

  for (var i = 0; i < data.responsetime.length; i++) {
    var responseTime = parseInt(data.responsetime[i].value);
    points.push([new Date(data.responsetime[i].datetime).getTime(), responseTime]);
    sum += responseTime;
  }

  $container.find(".response-time").text(Math.round(sum / data.responsetime.length) + "ms average response time");
});
...
{% endhighlight %}

At this point we have everything except a graph. I have found the most flexible graphing library to be [flot](http://www.flotcharts.org/). For this we need to add the base library and the following three plugins:

* **Threshold:** extends flot to change the colour of a line graph if the values reach above a certain value
* **Time:** handle time series data on graphs
* **Resize:** forces a rerender of the graphs on window resize


{% highlight html %}
<script type="text/javascript" src="/js/flot/jquery.flot.min.js"></script>
<script type="text/javascript" src="/js/flot/jquery.flot.time.min.js"></script>
<script type="text/javascript" src="/js/flot/jquery.flot.threshold.min.js"></script>
<script type="text/javascript" src="/js/flot/jquery.flot.resize.min.js"></script>
{% endhighlight %}

{% highlight javascript %}
...
getData(monitor, function (err, data) {
  ...
  var $graph = $container.find(".graph").removeClass("loading");
  $.plot($graph, [{
      data: points,
      color: "#F44336",
      shadowSize: 0,
      threshold: {
        below: monitor.threshold || 700,
        color: "#26ABE2"
      }
    }], {
    grid: {
      backgroundColor: null,
      borderColor: "rgba(0,0,0,0)",
      labelMargin: 10,
      margin: 0
    },
    xaxis: {
      mode: "time",
      min: window.moment().subtract(12, "hours").toDate(),
      max: window.moment().toDate()
    },
    yaxis: {
      max: monitor.max || 1000,
      min: 0
    }
  });
});
...
{% endhighlight %}

This creates a graph for our response time data over the last 12 hours. Now we have a complete status page with metrics managed with a Jekyll collection.

![Displaying the metrics](/images/blog/status-page/metrics.png){: .screenshot srcset="/images/blog/status-page/metrics.png 800w, /images/blog/status-page/metrics@2x.png 1600w"}

---

This status page is updatable using CloudCannon. This allows a team of non-developers to update the status page while the developers focus on the problems at hand. Alternatively it can still be updated through a storage provider and hosted anywhere static files can be served.

There are a number of alternatives to Uptimerobot with their own API. As an extension the data could be downloaded to the site as a JSON object which reduce the reliance on their uptime.

Feel free to use the status page template or contribute updates. If you have any questions comment below or on the repo. Remember to check out [status.cloudcannon.com](http://status.cloudcannon.com) and our [status twitter account](https://twitter.com/CCAppStatus) if you are experiencing any issues.