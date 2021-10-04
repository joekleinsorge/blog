---
title: 'How to Use Highcharts in a Hugo Blog'
date: 2021-09-28T15:58:20-04:00
draft: false
images:
tags:
  - Hugo
  - Highcharts
  - JavaScript
---

After viewing Janak's [expertise](https://janaksingh.com/#about) section, I knew I wanted to add some Highchart graphs to this blog. Janak detailed how he went about doing it in a [post](https://janaksingh.com/blog/hugo-add-highcharts/), but being new to Hugo and still learning how it works, his post left me with more questions.

After doing a bit more digging and actually reading Hugo's [documentation](https://gohugo.io/documentation/) (RTFM of course!). I learned that my preferred method for implementing [Highcharts](https://www.highcharts.com/) would be using Hugo's [shortcodes](https://gohugo.io/content-management/shortcodes/), just like how I am using [Codepen](https://github.com/JoeyKleinsorge/JoeyKleinsorge.com/blob/main/layouts/shortcodes/codepen.html) and [rawhtml](https://github.com/JoeyKleinsorge/JoeyKleinsorge.com/blob/main/layouts/shortcodes/rawhtml.html) for my Credly badges.

When I was looking around I found a Hugo theme for [highcharts](https://github.com/cmlnet/hugo-highcharts/blob/main/layouts/shortcodes/highcharts-custom.html) with the shortcode already setup. Anyways I shamelessly used his code to add my own "[highcharts-custom.html](https://github.com/JoeyKleinsorge/JoeyKleinsorge.com/blob/main/layouts/shortcodes/highcharts-custom.html)" shortcode.

In essence, to add some cool graphs to your Hugo website, add the following in an HTML file in the shorcode directory.

```html
<!--
  Based off of https://github.com/cmlnet/hugo-highcharts
-->

<!-- Load highcharts.js -->
{{ if not (.Page.Scratch.Get "highcharts_js") }}
  {{ if (fileExists "assets/js/highcharts.js") }}
    {{ $script := resources.Get "js/highcharts.js" }}
    {{ if hugo.IsProduction }}
      {{ $script = $script | minify | fingerprint }}
      <script type="text/javascript" src="{{ $script.RelPermalink }}" integrity="{{ $script.Data.Integrity }}"></script>
    {{ else }}
      <script type="text/javascript" src="{{ $script.RelPermalink }}"></script>
    {{ end }}
  {{ else }}
    <script src="https://code.highcharts.com/highcharts.js"></script>
  {{ end }}
  {{ .Page.Scratch.Set "highcharts_js" 1 }}
{{ end }}

<!-- Load highcharts-more.js -->
{{ if not (.Page.Scratch.Get "highcharts-more_js") }}
  {{ if (fileExists "assets/js/highcharts-more.js") }}
    {{ $script := resources.Get "js/highcharts-more.js" }}
    {{ if hugo.IsProduction }}
      {{ $script = $script | minify | fingerprint }}
      <script type="text/javascript" src="{{ $script.RelPermalink }}" integrity="{{ $script.Data.Integrity }}"></script>
    {{ else }}
      <script type="text/javascript" src="{{ $script.RelPermalink }}"></script>
    {{ end }}
  {{ else }}
    <script src="https://code.highcharts.com/highcharts-more.js"></script>
  {{ end }}
  {{ .Page.Scratch.Set "highcharts_js" 1 }}
{{ end }}

<!-- Place the chart -->
<div id="{{ .Get "chart" | default "chart" }}" style="min-width: 400px; max-width: 600px; height: 400px; margin: 0 auto"></div>

<!-- Script for the chart -->
<script>
  document.addEventListener('DOMContentLoaded', function () {
    var myChart = Highcharts.chart('{{ .Get "chart" }}', {
      {{ .Inner | safeJS }}
    });
  });
</script>
```

After you've set that up, in the page you want to add the chart to, just paste your chart JS inside of:

{{< gist JoeyKleinsorge 35495316dccae9a1fa71a64456481707 >}}

Once that's done can make cool chart like this:

{{< highcharts-custom chart="expertiseSpider" height="400" width="600" >}}
chart: {
polar: true,
type: 'area',
},
plotOptions: {
column: {
colorByPoint: true,
},
},
colors: ['#40a3b8', '#40a3b8'],
title: {
text: 'Cloud, Automation, Languages, Tooling',
},
pane: {
size: '80%',
},
xAxis: {
categories: [
'vRealize Suite',
'VMware',
'Dell MX7k',
'JavaScript',
'PowerShell',
'Kubernetes',
'Terraform',
'Docker',
'REST',
'AWS',
],
tickInterval: 0,
min: 0,
max: 10,
tickmarkPlacement: 'off',
lineWidth: 0,
},
yAxis: {
gridLineInterpolation: 'polygon',
lineWidth: 0,
min: 0,
},
tooltip: {
shared: true,
pointFormat: '{series.name}: <b>{point.y:,.0f}</b><br/>',
},
series: [
{
name: 'Experience',
data: [9,8,8,8,8,5,6,8,7,5],
pointPlacement: 'on',
//fillColor: 'Blue Gray',
fillOpacity: 0.6,
},
],
legend: {
enabled: false,
},
credits: {
enabled: false,
},
exporting: {
enabled: false,
},
{{< /highcharts-custom >}}
