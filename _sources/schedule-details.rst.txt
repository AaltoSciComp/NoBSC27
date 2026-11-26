:orphan:

Schedule
========




..
  .. jinja:: ctx1

   Summary
   -------

   {% for day_name, day in schedule.schedule|items%}

   {{day_name}}
   ------------------------------------------------------------------------------------------

   .. raw:: html

       {% for time, sessions in day|rejectattr("time","undefined")|groupby("time") %}
       {% for event in sessions|rejectattr("id", "undefined") %}

         <section id="{{event.get("id")}}">
         <h3>{{event.title}}
	   {%if 'location' in event and 'time' in event %}
	     ({{"%02d:%02d"|format(event.time//60, event.time%60)}}, {{schedule.meta.rooms[event.location].name}})
	   {% endif %}
	   {% if 'id' in event%}<a class="headerlink" href="#{{event.id}}" title="Link to this heading"></a>{% endif %}
	 </h3>
	 {% if 'contributors' in event %}<p>Contributors: {{event.get("contributors", "")}}</p> {% endif %}

         {{event.get("abstract") |markdown |remove_newlines }}
	 </section>
       {% endfor %}
       {% endfor%}
   {% endfor %}
