<html>
<head>
  <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
</head>

<body>
  <style type="text/css">
   a {
      color:#18c;
      text-decoration: none;
    }
  </style>

  {% if message %}
  <p>{{message}}</p>
  {% endif %}

  {% if ticket.parent_ticket != nil and recipient.is_admin %}
  <p>This ticket is sub-ticket of <a href="{{ticket.parent_ticket_url | escape }}">Ticket {{ticket.parent_ticket}}</a>: {{ticket.parent_ticket_summary | escape}}</p>
  {% endif %}
  {% if events contains 'ticket-closed-dup' %}
  <p>This ticket was closed as a duplicate of <a href="{% if recipient.is_admin %}{{ticket.master_ticket_url | escape}}{% else %}{{ticket.master_ticket_portal_url | escape}}{% endif %}">ticket #{{ticket.master_ticket | escape}}</a>.<br/>
  Please refer to that ticket for further comments and updates.</p>
  {% endif %}

  <p style="font-size:80%">From: {{ticket.last_comment.creator.full_name}} &lt;{{ticket.last_comment.creator.email}}&gt;<br/>
  {% if recipient.full_name_or_email != ticket.creator.full_name_or_email and ticket.last_comment.creator.full_name_or_email != ticket.creator.full_name_or_email %}Ticket Creator: {{ticket.creator.full_name}} &lt;{{ticket.creator.email}}&gt;<br/>{% endif %}
  {% assign cc = 'no' %} 
  {% for user in ticket.users %} 
  {% if user.full_name_or_email != ticket.assignee.full_name_or_email and user.full_name_or_email != ticket.creator.full_name_or_email %} 
  {% if cc == 'no' %}CC: {% assign cc = 'yes' %}{%endif%}{{user.full_name}} &lt;{{user.email}}&gt;{% if forloop.last != true and forloop.length > 2 %}, {%endif%}
  {%endif%}
  {%endfor%}
  </p>

  {{ticket.last_comment.body}}

  <br/><br/>

  {% if recipient.is_admin %}
  <hr style="border:0;height:1px;background:#333;"/>
    <div>
      <h3>Ticket Overview</h3>
      <a href="{{ticket.url| escape}}">View in Browser</a>
      <p style="font-size:80%;">Creator: {{ticket.creator.full_name_or_email | escape}}<br/>
         Assignee: {{ticket.assignee.full_name_or_email | escape}}</p>
      </ul>
    {% if ticket.has_subtickets %}
      <div>
        <h3>Subtickets</h3>
        <ul>
        {% for subticket in ticket.subtickets %}
          <li><a href="{{subticket.url}}">{{subticket.ref}}</a>
            <span>{{subticket.status}}</span>
            <span>{{subticket.summary | escape}}</span>
          </li>
        {% endfor %}
        </ul>
      </div>
    {% else %}
    {% endif %}
      <p><a href="https://community.spiceworks.com/support/help-desk/docs/working-the-help-desk#command-list">List of Ticket Commands</a><br/>
      Examples: <code>#close, #add 5m, #assign to bob, #priority high</code></p>
    </div>
  {% endif %}

  {% if ticket.previous_comments != empty %}
    <hr style="border:0;height:1px;background:#333;"/>
    <h3>Ticket History</h3>
  {%endif%}

  {% for comment in ticket.previous_comments %}
    <p style="font-style:italic;">On {{comment.created_at | date_sw}}, {{comment.creator.full_name_or_email}} wrote:</p>

    {{comment.body}}
    <br/><br/>

    <hr style="border:0;height:1px;background:#333;"/>
  {% endfor %}

  {% unless recipient.is_admin %}
    {% if ticket.previous_comments == empty %}<hr style="border:0;height:1px;background:#333;"/>{% endif %}

    {% if ticket.suggested_kb_articles.size > 0 %}
    <p>While you wait, see if the following help articles are related to your issue:</p>
    <ul>
    {% for article in ticket.suggested_kb_articles %}
      <li><a href="{{article.url}}">{{article.title}}</a></li>
    {% endfor %}
    </ul>
    {% endif %}

  {% endunless %}
</body>
</html>
