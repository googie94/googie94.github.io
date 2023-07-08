---
layout: default
title: "Daily"
description: 매일을 기록해요
main: true
project-header: true
header-img: img/about.jpg
lastmod: 2023-03-04 2:00:00
---
<script src='https://cdn.jsdelivr.net/npm/@fullcalendar/core@6.1.4/index.global.min.js'></script>
<script src='https://cdn.jsdelivr.net/npm/@fullcalendar/daygrid@6.1.4/index.global.min.js'></script>\
<script src="https://cdn.jsdelivr.net/npm/tooltip-js@3.0.0/dist/tooltip-min.min.js"></script>
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
<script>
document.addEventListener('DOMContentLoaded', function() {
	function htmlDecode(input){
	  var e = document.createElement('div');
	  e.innerHTML = input;
	  return e.childNodes[0].nodeValue;
	}
	var calendarEl = document.getElementById('calendar');
	var calendar = new FullCalendar.Calendar(calendarEl, {
		firstDay: 1,
		// expandRows: true,
		timeZone: 'Asia/seoul',
		// dayMaxEvents: true,
		locale: 'ko',
		initialView: 'dayGridMonth',
		eventTimeFormat: {
			hour: '2-digit',
			hour12: false
		},
		eventDidMount: function(event, element) {
			event.el.style.fontSize = "12px";
			if (event.event._def.extendedProps.is_success == 0){
				event.el.firstChild.style.border = "4px solid #FF4D37";
			}
			if (event.event._def.extendedProps.is_success == 1) {
				event.el.firstChild.style.border = "4px solid #00CB7F";
			}
		},
		eventMouseEnter: function(event) {
			var tis=event.el;
			var tooltip = htmlDecode('<div class="tooltipevent" style="top:'+($(tis).offset().top-5)+'px;left:'+($(tis).offset().left+($(tis).width())/2)+'px"><div>'+event.event._def.title+'</div>');
			var $tooltip = $(tooltip).appendTo('body');
		},
		eventMouseLeave: function(info) {
			$(info.el).css('z-index', 8);
			$('.tooltipevent').remove();
		},
		events: {{ site.data.daily | replace: '=>', ':' }}
	});
	calendar.setOption('height', 450);
	calendar.render();
});
</script>

<div id='calendar'></div>

<ul class="catalogue">
{% assign sorted = site.pages | sort: 'order' | reverse %}
{% for page in sorted %}
{% if page.daily == true %}
{% include post-list.html %}
{% endif %}
{% endfor %}
</ul>