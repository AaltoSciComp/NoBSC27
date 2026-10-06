Schedule
========


.. jinja:: ctx1

   Timetable
   ---------

   .. raw:: html

     {% for day_name, day in schedule.schedule|items%}
       <h3>{{ schedule.meta.days[day_name] }}</h3>

       <table class="docutils" style="word-wrap: break-word;">
       <tr>
          <th style="width: 5%"></th>
          {% for room_name, room_data in schedule.meta.rooms|items %}
            <th style="vertical-align: top; width: 20%">{{room_data.name}}
	    {% if 'description' in room_data %}<br><span style="font-weight: normal;">{{room_data.description}}</span>{% endif %}
            </th>
          {% endfor %}
       </tr>

       {% for time, sessions in day|rejectattr("time","undefined")|groupby("time") %}
          <tr>
          <th>{{"%02d:%02d"|format(time//60, time%60)}}</th>

          {% for room in schedule.meta.rooms %}

            <td>
            {% for event in sessions|rejectattr("location", "undefined")|selectattr("location", "eq", room) %}
               <b>
                 {% if 'id' in event %}<a
		 href="../sessions/#{{event.id}}">{{ event.shorttitle|default(event.title) }}</a>
                 {% else %}{{ event.shorttitle|default(event.title) }}
                 {% endif %}
               </b>
               {% if 'short' in event %}
                 <br>{{ event.short }}
               {% endif %}
               {% if 'contributors' in event %}
                 <br><small>{{ event.contributors }}</small>
               {% endif %}
               {%if not loop.last %}<br><br>{% endif %}
            {% endfor %}
            </td>

          {% endfor %}
          </tr>

       {% endfor %}
       </table>

     {% endfor %}
