---
layout: schedule
permalink: /schedule/
title: Schedule
---

{% assign current_module = 0 %}
{% assign skip_classes = 0 %}
{% assign prev_date = 0 %}

{% for item in site.data.schedule %}
{% if item.date %}
{% assign lecture = item %}
{% assign event_type = "upcoming" %}
{% assign today_date = "now" | date: "%s" | divided_by: 86400 %}
{% assign lecture_date = lecture.date | date: "%s" | divided_by: 86400 %}
{% if today_date > lecture_date %}
    {% assign event_type = "past" %}
{% elsif today_date <= lecture_date and today_date > prev_date %}
    {% assign event_type = "warning" %}
{% endif %}
{% assign prev_date = lecture_date %}

<tr class="{{ event_type }}">
    <th scope="row">{{ lecture.date }}</th>
    {% if lecture.title %}
        {% if lecture.title contains 'No class' or forloop.last %}
        {% assign skip_classes = skip_classes | plus: 1 %}
        <td colspan="4" align="center">{{ lecture.title }}</td>
        {% else %}
        <td>
            Lecture #{{ forloop.index | minus: current_module | minus: skip_classes }}
            {% if lecture.lecturer %}({{ lecture.lecturer }}){% endif %}:
            <br />
            {{ lecture.title }}
            <br />
        </td>
        <td>
            {% if lecture.readings %}
            <ul style="list-style-type: none; padding: 0;">
            {% for reading in lecture.readings %}
                <li style="margin-bottom: 20px; padding-bottom: 10px; border-bottom: 1px solid #ccc;">
                    {% if reading.url %}
                        <strong style="font-size: 1.1em;"><a href="{{ reading.url }}" style="text-decoration: none; color: #2c3e50;">{{ reading.title }}</a></strong>
                    {% else %}
                        <strong style="font-size: 1.1em; color: #2c3e50;">{{ reading.title }}</strong>
                    {% endif %}
                    {% if reading.authors %}
                        <div style="font-size: 0.9em; color: #7f8c8d; margin-top: 5px;">{{ reading.authors }}</div>
                    {% endif %}
                    {% if reading.year %}
                        <div style="font-size: 0.8em; color: #95a5a6; margin-top: 3px;">{{ reading.year }}</div>
                    {% endif %}
                </li>
            {% endfor %}
            </ul>
            {% endif %}
        </td>
        <td>
            <p>{{ lecture.logistics }}</p>
        </td>
        {% endif %}
    {% else %}
        {% assign skip_classes = skip_classes | plus: 1 %}
        <td colspan="4" align="center">{{ lecture.logistics }}</td>
    {% endif %}
</tr>
{% else %}
{% assign current_module = current_module | plus: 1 %}
{% assign module = item %}
<tr class="info">
    <td colspan="5" align="center"><strong>{{ module.title }}</strong></td>
</tr>
{% endif %}
{% endfor %}
