<td>
    <span class="status {{ box.status }}">{{ box.status }}</span>
</td>
<td>
    <a href="/boxes/{{ box.box_id }}/history/view">View History</a><br><br>

    {% if box.status == "ready_for_qa" %}
    <form action="/dashboard/qa_approve/{{ box.box_id }}" method="post" style="display:inline;">
        <button type="submit">Approve</button>
    </form>
    <form action="/dashboard/qa_reject/{{ box.box_id }}" method="post" style="display:inline;">
        <button type="submit">Reject</button>
    </form>
    {% elif box.status == "qa_passed" or box.status == "ready_for_delivery" %}
    <form action="/dashboard/delivered/{{ box.box_id }}" method="post" style="display:inline;">
        <button type="submit">Mark Delivered</button>
    </form>
    {% else %}
    -
    {% endif %}
</td>
