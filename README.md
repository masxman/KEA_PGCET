# KEA PGCET — University Marks Table Fix

A common issue on the **KEA PGCET online application portal** where the *University Marks Details* table fails to render Year 1, Year 2, and Year 3 rows, leaving candidates unable to enter their marks.

---
*Disclaimer: I am not responsible for any issue, data loss, or consequences arising from the use of this script.
Use it on your own risk*
## The Problem

On the **University Marks Details** page of the KEA PGCET application form, the marks entry table may appear blank or incomplete — the rows for Year 1, Year 2, or Year 3 are simply not generated. This prevents applicants from filling in their semester/year-wise marks.

---

## The Fix (Browser Console Method)

### Step 1 — Open the Marks Details Page

Log in to your KEA PGCET account and navigate to the **University Marks Details** page where the table should appear.

---

### Step 2 — Open the Browser Console

| Browser | Shortcut |
|---|---|
| Chrome / Edge | `F12` → **Console** tab |
| Firefox | `F12` → **Console** tab |
| Safari | yeah you are on your own, or just google idk. |

---

### Step 3 — Paste and Run the Code

Click inside the console input field, paste the following code, and press **Enter**:

```javascript
var tbody = document.getElementById('marksBody');
var templateRow = tbody.querySelector('tr[data-yr="3"]').cloneNode(true);
tbody.innerHTML = '';
[1, 2, 3].forEach(year => {
    var newRow = templateRow.cloneNode(true);
    newRow.setAttribute('data-yr', year);
    var firstCell = newRow.querySelector('td');
    if (firstCell) firstCell.innerText = year;
    var inputs = newRow.querySelectorAll('input, select');
    inputs.forEach(input => {
        input.value = '';
        input.setAttribute('data-val', 'true');
    });
    tbody.appendChild(newRow);
});
console.log("SUCCESS: Year 1, 2, and 3 rows added.");
```

You should see the message **`SUCCESS: Year 1, 2, and 3 rows added.`** printed in the console, and the table rows will now be visible on the page.

---

### Step 4 — Enter Your Marks

Fill in all your marks for **Year 1**, **Year 2**, and **Year 3** in the now-visible table rows.

---

### Step 5 — Save and Submit

Click the **Save** or **Next** button on the page to submit your marks to the server as you normally would.

---

## Verifying That Your Data Was Saved

To confirm your marks have been saved correctly:

1. **Log out** of your KEA PGCET account completely.
2. Open a **new browser window** (or use a different browser / Incognito mode).
3. **Log back in** with your credentials.
4. Navigate back to the **University Marks Details** page.
5. ✅ If your marks are pre-filled, the data was saved successfully.
6. ❌ If the fields are empty again, try the steps above once more and ensure you clicked **Save/Submit** before logging out.

---

## Why This Works

The page relies on a Year 3 row being present in the DOM as a template to clone for Years 1 and 2. Due to a rendering bug, all three rows may fail to generate. This script manually rebuilds the three rows using the correct `data-yr` attributes that the server expects, allowing your input to be submitted normally.

---

## Notes

- This fix works **client-side only** — it does not modify the website permanently.
- You must **save/submit the form** after entering marks; simply closing the tab will not persist data.
- This has been tested on the KEA PGCET portal. If the portal is updated, the fix may not work.
- If `tbody.querySelector('tr[data-yr="3"]')` returns `null`, the page structure may have changed — raise an issue in this repository.

---

## Contributing

If you found a variation of this issue or a better fix, feel free to open a Pull Request or raise an Issue.

---

*This repository is not affiliated with KEA or the Government of Karnataka. It is a community-maintained fix shared for the benefit of PGCET applicants.*
