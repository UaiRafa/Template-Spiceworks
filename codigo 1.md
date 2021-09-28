<body style="margin:0px; padding:0px; border: 0 none; font-size: 11px; font-family: Verdana, sans-serif; background-color: #efefef;">

<table style="width: 700px; margin: 50px auto 50px auto; border: 1px #fff solid; background: #fff; font-size: 12px; font-family: Verdana, sans-serif;" align="center">
<tbody>
<tr>

<td colspan="2" style="padding: 9px; color: #fff; background: #990000;"><h1>Eficaz Consultoria - chamado</h1></td>     
      
</tr>
<tr>	
	<tr style="padding: 10px;">
		<td style="width: 400px; padding: 18px;"><p>Olá {{recipient.full_name_or_email}},</p>
			<tr>
	<!-- ************************************************************************** -->	
	{% case event %}
	<!-- ************************************************************************** -->	
	{% when 'ticket-opened' %}
		{% case recipient.role %}
			{% when 'end_user' %}
				<p>Seu chamado foi recebido pelo TI e logo será respondido.</p>
			{% when 'admin' %}
				<p>Um novo chamado foi criado.</p>
		{% endcase %}
	<!-- ************************************************************************** -->	
	{% when 'ticket-assigned' %}
		{% case recipient.role %}
			{% when 'end_user' %}
				<p>Em {{ticket.created_at | date_sw}} seu chamado foi criado e será encaminhado para um técnico. <strong>{{ticket.assignee.full_name_or_email}}</strong>.</p>
			{% when 'admin' %}
				<p>Você foi designado ou você aceita o ticket &#35;{{ticket.id}} criado em {{ticket.created_at | date_sw}}.</p>
		{% endcase %}
	<!-- ************************************************************************** -->	
	{% when 'ticket-comment' %}
		<p>Em {{ticket.last_comment.created_at | date_sw }} {{ticket.last_comment.creator.full_name_or_email}} escreveu:</p>
		<q style="font-style: italic; font-weight: bold; quotes: '„' '”';">
		{{ticket.last_comment.body | escape | simple_format | replace:'<br /><br />','<br />'}}</q>
	<!-- ************************************************************************** -->	
	{% when 'ticket-closed' %}
		{% case recipient.role %}
			{% when 'end_user' %}
				<p>em {{ticket.closed_at | date_sw}} o seu chamado foi finalizado, o que significa que o seu problema foi resolvido. Não responda a este e-mail. Abaixo, histórico do chamado:</p>
			{% when 'admin' %}
				<p>chamado &#35;{{ticket.id}} foi finalizado em {{ticket.closed_at | date_sw}}. Abaixo você encontrará o histórico completo do chamado.</p>
		{% endcase %}
	<!-- ************************************************************************** -->	
	{% when 'ticket-reopened' %}
		{% case recipient.role %}
			{% when 'end_user' %}
				<p>O seu chamado foi reaberto. Por favor, certifique-se de ter introduzido a razão pela qual reabriu o chamado. Se não, por favor, clique no link na caixa à direita e atualize o seu chamado com motivo da reabertura. Você também pode fechar o chamado se tiver reaberto por engano. Abaixo está a história do chamado completo.</p>
			{% when 'admin' %}
				<p>chamado &#35;{{ticket.id}} foi reaberto.</p>
		{% endcase %}
	<!-- ************************************************************************** -->	
	{% endcase %}
	<!-- ************************************************************************** -->	
		</tr>
	<br/>
	<div style="padding: 2px; border: 1px #ccc solid; background: #eee;">
		<div style="margin: 9px;">
			<h4>chamado</h4>
			<p>Título: {{ticket.summary | escape}}</p>
			<p>Conteúdo:</p>
				{{ticket.body | escape | simple_format | replace:'<br /><br />','<br />'}}
		</div>
	</div>

	<!-- ************************************************************************** -->	
	{% case event %}
	<!-- ************************************************************************** -->	
	{% when 'ticket-opened' %}
        <!-- If you have specific items that you want to display in certain circumstances, -->
        <!-- prior to the ticket summary.  I wanted a summary on each email that went out -->


	<!-- ************************************************************************** -->	
	{% when 'ticket-assigned' %}


	<!-- ************************************************************************** -->	
	{% when 'ticket-comment' %}
	
	
	<!-- ************************************************************************** -->	
	{% when 'ticket-closed' %}


	<!-- ************************************************************************** -->	
	{% when 'ticket-reopened' %}


	<!-- ************************************************************************** -->	
	{% endcase %}
	<!-- ************************************************************************** -->	
</tr>
		<br/>
		{% if ticket.previous_comments != empty %}
			<div style="padding: 2px; border: 1px #ccc solid; background: #eee;">
				<h4>Histórico</h4>
				<hr style="height: 1px; color: #ccc;" />
				{% for comment in ticket.previous_comments %}
					<p>Em {{comment.created_at | date_sw }} {{comment.creator.full_name_or_email}} escreveu:<br />
					<q style="font-size: smaller; font-style: italic; font-weight: bold; quotes: '„' '”';">
					{{comment.body | escape | simple_format | replace:'<br /><br />','<br />'}}
					</q></p>
					<hr style="height: 1px; color: #ccc;" />
				{% endfor %}
			</div>
		{% endif %}

		<br />
		<p>Att,<br />
		Meu Nome Completo </p>
		</td>
		
		<td style="width: 240px; vertical-align: top;"><div style="padding: 9px; border: 1px #ccc solid; font-size: 10px;">chamado Nº: <strong>&#35;{{ticket.id}}</strong>
		<hr style="height: 1px; color: #ccc;" />
		Data: <strong>{{ticket.created_at | date_sw}}</strong><br />
		Aberto por: <strong>{{ticket.creator.full_name_or_email}}</strong><br />
		Título: <strong>{{ticket.summary}}</strong><br />
		Prioridade: <strong>{{ticket.priority}}</strong><br />
		chamado Link: <strong><a style="color:#FF6600; text-decoration: none;" href="
			{% if recipient.role == 'admin' %}
				{{ticket.url}}
			{% else %}
				{{ticket.portal_url}}
			{% endif %}">Clique aqui</a></strong><br/>
		Designado para: <strong>{{ticket.assignee.full_name_or_email}}</strong>

		<hr style="height: 1px; color: #ccc; margin: 0;" />

		<p style="text-align: justify;">Se você tiver qualquer informação adicional sobre este problema, responder a este chamado. Por favor, lembre-se de manter o <strong>{{ticket.ref}}</strong> atualizado acessando este chamado pelo link: <a style="color:#FF6600; text-decoration: none;" href="
			{% if recipient.role == 'admin' %}
				{{ticket.url}}
			{% else %}
				{{ticket.portal_url}}
			{% endif %}
		" >Clique aqui.</a></p>
		</div>
		</td>
	</tr>
	<tr>
		<td colspan="2" style="background: #ddd; padding: 9px; margin-top: 18px; text-align: center;">&copy; Eficaz Consultoria Ltda</td>
	</tr>
</tbody>
</table>
</body>
</html>
