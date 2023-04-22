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
<script src='https://cdn.jsdelivr.net/npm/@fullcalendar/daygrid@6.1.4/index.global.min.js'></script>
<script>
document.addEventListener('DOMContentLoaded', function() {
	var calendarEl = document.getElementById('calendar');
	var calendar = new FullCalendar.Calendar(calendarEl, {
		firstDay: 1,
		expandRows: true,
		timeZone: 'Asia/seoul',
		dayMaxEvents: true,
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
		events: {{ site.data.daily | replace: '=>', ':' }}
	});
	if (document.body.clientWidth < 800) {
		calendar.setOption('height', 350);
	}
	else {
		calendar.setOption('height', 700);
	}
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