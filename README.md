<th>Action</th> 


<td>
    <form action="/scanner/mark_ready/{{ box.box_id }}" method="post">hh
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


from fastapi.responses import RedirectResponse


@app.post("/scanner/mark_ready/{box_id}")
def mark_ready(box_id: str):
    cursor.execute(
        """
        UPDATE boxes
        SET status = %s
        WHERE box_id = %s
        """,
        ("ready_for_index", box_id)
    )
    conn.commit()

    return RedirectResponse(url="/scanner", status_code=303)




@app.post("/indexing/send_to_qa/{box_id}")
def send_to_qa(box_id: str):
    cursor.execute(
        """
        UPDATE boxes
        SET status = %s
        WHERE box_id = %s
        """,
        ("ready_for_qa", box_id)
    )
    conn.commit()

    return RedirectResponse(url="/indexing", status_code=303)


    <td>
    <form action="/indexing/send_to_qa/{{ box.box_id }}" method="post">
        <button type="submit">Send to QA</button>
    </form>
</td>
