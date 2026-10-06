List of sessions
================

This list currently only has the pre-planned "keynote" sessions
invited by the organizers for the SciComp meetup (the second half).


.. jinja:: ctx1


   {% for day_name, day in schedule.schedule|items%}

   {{ schedule.meta.days[day_name] }}
   ------------------------------------------------------------------------------------------

   .. raw:: html

       {% for time, sessions in day|rejectattr("time","undefined")|groupby("time") %}
       {% for event in sessions|rejectattr("id", "undefined") %}

         <section id="{{event.get("id")}}">
         <h3 style="margin-bottom: 0px;">{{event.title}}
	   {% if 'id' in event%}<a class="headerlink" href="#{{event.id}}" title="Link to this heading"></a>{% endif %}
	 </h3>
	 <ul>
	 {% if 'contributors' in event %}<li><i>Contributors: {{event.get("contributors", "")}}</i></li>{% endif %}
	 {%if 'location' in event and 'time' in event %}<li><i>Time, Location: {{"%02d:%02d"|format(event.time//60, event.time%60)}}, {{schedule.meta.rooms[event.location].name}}</i></li>{% endif %}
	 </ul>

         {{event.get("abstract") |markdown |remove_newlines }}
	 </section>
       {% endfor %}
       {% endfor%}
   {% endfor %}


