<th>Action</th> 


<td>
    <form action="/scanner/mark_ready/{{ box.box_id }}" method="post">
        <button type="submit">Send to Indexing</button>
    </form>
</td>


@app.post("/scanner/mark_ready/{box_id}")
def mark_ready(box_id: str):
    cursor.execute(
        "UPDATE boxes SET status = %s WHERE box_id = %s",
        ("ready_for_index", box_id)
    )
    conn.commit()

    return RedirectResponse(url="/scanner", status_code=303)
