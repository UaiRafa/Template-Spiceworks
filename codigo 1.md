html>
<body style="margin:0px; padding:0px; border: 0 none; font-size: 12px; font-family:   Verdana; background-color: #efefef;">

<table style="width: 95%; margin: 50px auto 50px auto; border: 1px #009EE7 solid; background: #fff; font-size: 13px; font-family:  Verdana;" align="center">
<tbody>
	<tr>
        <td colspan="2" style="border:0px;padding: 9px; color: #fff; background: #009EE7;"><h1 style="margin-bottom:0px;">IT Helpdesk - Ticket &#35;{{ticket.id}}</h1>
            <h2 style="margin-bottom:0px;">{% case event %}{% when 'Chamado aberto' %}Chamado aberto{% when 'Chamado Reaberto' %}Chamado Reaberto{% when 'Chamado designado' %}Chamado designado ({{ticket.assignee.full_name | escape}}) {% when 'Comentário do chamado' %}Chamado atualizado{% when 'Chamado Fechado' %}Chamado Fechado{% endcase %} - {{ticket.summary | escape}}</h2>
        </td>
	</tr>
	
	<tr style="padding: 10px;">
		<td colspan="2" style="width: 90%; padding: 15px 0px 0px 18px; vertical-align: top;"><p>{{recipient.full_name_or_email}},</p>

<!-- ************************************************************************** -->	
{% case event %}
<!-- ************************************************************************** -->	
	{% when 'Chamado aberto' %}
		{% case recipient.role %}
			{% when 'end_user' or 'reporting' %}
				<p>O Chamado foi recebido pela equipe de TI. Por favor, atente-se as atualizações via E-mail, algum de nós dará o retorno.</p>
				<p>Se você tiver alguma informação adicional sobre este chamado; responda este E-mail. Por favor mantenha <strong>{{ticket.ref}}</strong> no assunto do E-mail.</p>
			{% when 'admin' or 'helpdesk_admin' %}
				<p>Um novo chamado foi adicionado pela equipe de TI.</p>
				<p>Se você tiver alguma informação adicional sobre este chamado; responda este E-mail. Por favor mantenha <strong>{{ticket.ref}}</strong> no assunto do E-mail.</p>
		{% endcase %}
<!-- ************************************************************************** -->	
	{% when 'Chamado designado' %}
		{% case recipient.role %}
			{% when 'end_user' or 'reporting' %}
			{% if ticket.assignee.full_name_or_email == null %}
			<p style="font-style: italic;">On {{ticket.last_comment.created_at | date_sw}},</p><p style="font-style: italic;">Ticket &#35;{{ticket.id}} was unassigned.</p>
			{% else %}
				<p>On {{ticket.last_comment.created_at | date_sw}},</p>
          <p>Ticket &#35;{{ticket.id}} was assigned to <strong>{{ticket.assignee.full_name_or_email}}</strong>.  For further assistance please contact them by email: {{ticket.assignee.email}}
{% if ticket.assignee.office_phone == null %}
  {% if ticket.assignee.cell_phone == null %}
	.
  {% else %}
	, on their cellphone: {{ticket.assignee.cell_phone}}.
  {% endif %}
{% else %}
	, on their office phone: {{ticket.assignee.office_phone}}
  {% if ticket.assignee.cell_phone == null %}
	.
  {% else %}
	, or on their cellphone: {{ticket.assignee.cell_phone}}.
  {% endif %}
{% endif %}</p> 
			{% endif %}
				{% if events contains 'Comentário do chamado' %}<p style="font-style: italic;">Com o comentário:</p>
		<p>{{ticket.last_comment.body | escape | simple_format | replace:'<br /><br />','<br />'}}</p>
{% endif %}
			{% when 'admin' or 'helpdesk_admin' %}
				<p style="font-style: italic;">Ticket &#35;{{ticket.id}} created on {{ticket.created_at | date_sw}} Foi Designado a: <strong>{{ticket.assignee.full_name_or_email}}</strong>.</p>
								{% if events contains 'Comentário do chamado' %}<p style="font-style: italic;">Com o comentário:</p>
		<p>{{ticket.last_comment.body | escape | simple_format | replace:'<br /><br />','<br />'}}</p>
			{% endif %}
		{% endcase %}
<!-- ************************************************************************** -->	
	{% when 'Comentário do chamado' %}
		<p style="font-style: italic;">On {{ticket.last_comment.created_at | date_sw }}, <strong>{{ticket.last_comment.creator.full_name_or_email}}</strong> wrote:</p>
		<p>{{ticket.last_comment.body | escape | simple_format | replace:'<br /><br />','<br />'}}</p>
<!-- ************************************************************************** -->	
	{% when 'Chamado Fechado' %}
		{% case recipient.role %}
			{% when 'end_user' or 'reporting' %}
				<p style="font-style: italic;">On {{ticket.closed_at | date_sw}},</p><p style="font-style: italic;">chamado &#35;{{ticket.id}} Foi Fechado.</p>
				{% if events contains 'Comentário do chamado' %}<p style="font-style: italic;">Com o comentário:</p>
		<p>{{ticket.last_comment.body | escape | simple_format | replace:'<br /><br />','<br />'}}</p>
			{% endif %}
			<p style="font-style: italic;">Por favor, só responda este E-mail se o chamado não foi fechado com sua satisfação. Caso queira agradecer <strong>{{ticket.assignee.full_name_or_email}}</strong>, Use o link deste email.</p><p> We need your help! <strong> Please help us in improving our level of service by completing the following anonymous survey.</strong>(https://www.surveymonkey.com/r/5RXGWPF)
			{% when 'admin' or 'helpdesk_admin' %}
				<p style="font-style: italic;">Ticket &#35;{{ticket.id}} was closed on {{ticket.closed_at | date_sw}}.</p>
								{% if events contains 'Comentário do chamado' %}<p style="font-style: italic;">Com o comentário:</p>
		<p>{{ticket.last_comment.body | escape | simple_format | replace:'<br /><br />','<br />'}}</p>
			{% endif %}
		{% endcase %}
<!-- ************************************************************************** -->	
	{% when 'Chamado Reaberto' %}
		{% case recipient.role %}
			{% when 'end_user' or 'reporting' %}
				<p style="font-style: italic;">Chamado &#35;{{ticket.id}} Foi reaberto.</p>
{% if events contains 'Comentário do chamado' %}<p style="font-style: italic;">Com o comentário:</p>
		<p>{{ticket.last_comment.body | escape | simple_format | replace:'<br /><br />','<br />'}}</p>
			{% endif %}
			 <p style="font-style: italic;">If the reason is not listed above, please click on the Ticket URL in the box to the right and update the ticket with the reason.  You may also close the ticket from there if you have re-opened it in error.</p>
				{% when 'admin' or 'helpdesk_admin' %}
				<p style="font-style: italic;">Chamado &#35;{{ticket.id}} Foi reaberto.</p>
				{% if events contains 'Comentário do chamado' %}<p style="font-style: italic;">Com o comentário:</p>
		<p>{{ticket.last_comment.body | escape | truncatewords: 100 | simple_format | replace:'<br /><br />','<br />'}}</p>
			{% endif %}
		{% endcase %}
<!-- ************************************************************************** -->	
{% endcase %}
<!-- ************************************************************************** -->	
</tr>
<tr style="padding: 10px;">
<td colspan=2 style="vertical-align: top;"><div style="margin:15px 8px 15px;padding: 9px; border: 2px #009EE7 solid; font-size: 12px;"><strong>TICKET &#35;: </strong><font style="color:#009EE7;">{{ticket.id}}</font>
	<hr style="height: 1px; color: #ccc;" />
	<strong>Data de criação: </strong><font style="color:#009EE7;">{{ticket.created_at | date_sw}}</font><br /><br />
    <strong>Data de vencimento: </strong><font style="color:#009EE7;">{{ticket.due_at | date_sw}}</font><br /><br />
  	<strong>Solicitante: </strong><a style="color:#009EE7; text-decoration: none;" href="mailto:{{ticket.creator.email}}">{{ticket.creator.full_name_or_email}}</a><br /><br />
	<strong>Resumo: </strong><font style="color:#009EE7;">{{ticket.summary}}</font><br /><br />
  <strong>Prioridade: </strong><font style="color:#009EE7;">{{ticket.priority}}</font><br /><br />
	<strong>Ticket URL: </strong><a style="color:#009EE7; text-decoration: none;" href="
		{% if recipient.role == 'admin' or recipient.role == 'helpdesk_admin' %}
			{{ticket.url}}
		{% else %}
			{{ticket.portal_url}}
		{% endif %}">Ticket &#35;{{ticket.id}}</a><br/><br />
	<strong>Assignee: </strong><a style="color:#009EE7; text-decoration: none;" href="mailto:{{ticket.assignee.email}}">{{ticket.assignee.full_name_or_email}}</a><br />
  {% if ticket.assignee.office_phone == null %}
  {% if ticket.assignee.cell_phone == null %}
  {% else %}
	<div style="margin-left: 70px; color:#009EE7;">Cellphone: {{ticket.assignee.cell_phone}}</div>
  {% endif %}
{% else %}
  <div style="margin-left: 70px; color:#009EE7;">Office phone: {{ticket.assignee.office_phone}}<br>
  {% if ticket.assignee.cell_phone == null %}
  {% else %}
	Cellphone: {{ticket.assignee.cell_phone}}</div>
  {% endif %}
{% endif %}<br />

	<strong>Description: </strong><font style="color:#009EE7;">{{ticket.body | escape | truncatewords: 100 | simple_format | replace:'<br /><br />','<br />'}}</font>
	<hr style="height: 1px; color: #ccc;" />

	<p style="margin-top:5px;">If you have any additional information regarding this ticket respond to this email. Please remember to keep <strong>{{ticket.ref}}</strong> in the email subject. You can also log into the Help Desk system <a style="color:#FF6600; text-decoration: none;" href="
		{% if recipient.role == 'admin' or recipient.role == 'helpdesk_admin' %}
			{{ticket.url}}
		{% else %}
			{{ticket.portal_url}}
		{% endif %}
	" >here</a> para adicionar um comentário e ver outros chamados.</p>
	</div>
	</td>
	</tr>
<!-- ************************************************************************** -->	
{% case event %}
<!-- ************************************************************************** -->	
	{% when 'Chamado aberto' %}
        <!-- Se você tiver itens específicos que deseja exibir em certas circunstâncias, -->
        <!-- antes do resumo do Chamado. Eu gostaria de um resumo de cada e-mail enviado -->


<!-- ************************************************************************** -->	
	{% when 'Responsável pelo chamado' %}


<!-- ************************************************************************** -->	
	{% when 'Comentários do chamado' %}
	
	
<!-- ************************************************************************** -->	
	{% when 'Chamado fechado' %}


<!-- ************************************************************************** -->	
	{% when 'Chamado Reaberto' %}


<!-- ************************************************************************** -->	
{% endcase %}
<!-- ************************************************************************** -->	
</td>
</tr>
{% if ticket.previous_comments != empty %}
<tr style="padding: 10px;">
    <td colspan="2" style="vertical-align: top;">
			<div style="padding: 9px; margin:15px 8px 15px; border: 2px #009EE7 solid; background: #eee;">
				<h2 style="font-size:12px;">Ticket History</h2>
				{% for comment in ticket.previous_comments %}
				<hr/>
					<p style="margin-top:2px; font-style: italic;color: #606060;">On {{comment.created_at | date_sw }} {{comment.creator.full_name_or_email}} wrote:<br />
					<q style="quotes: '�' '�';">
					{{comment.body | escape | truncatewords: 50 | simple_format | replace:'<br /><br />','<br />'}}
					</q></p>

				{% endfor %}
			</div>
		</td>	
	</tr>
		{% endif %}
</tbody>
</table>
</body>
</html>