# Kai Ika Daily Report – README

## Overview

The **Kai Ika Daily Report** is a lightweight web application designed to track daily sales, product movements, and staff for the Kai Ika Project. It allows users to:

* Enter daily summary data (total sales, bins generated/distributed, who completed the form).
* Record individual product sales with quantities.
* Record staff activity with tasks, start/end times, and automatically calculate hours worked.
* Submit all this data directly to a Google Sheet for easy storage and analysis.

The app is **browser-based**, responsive, and works on both desktop and mobile devices. It uses **React** for interactivity and **Tailwind CSS** for styling, with a **Google Apps Script backend** for saving the data to Google Sheets.

---

## Key Features

### 1. Row Types

Data is organized into **three types of rows**:

* **DAY**: Contains the daily summary (date, total sales, bins generated/distributed, who completed the form).
* **PRODUCT**: Records the name and quantity of products sold or processed.
* **STAFF**: Records staff name, task, and start/end times, with automatic calculation of hours worked.

Users can add multiple product and staff rows as needed.

### 2. User-Friendly Interface

* **Section backgrounds**: DAY, PRODUCT, and STAFF rows have distinct colors for clarity.
* **Remove row button**: Each row has a small `✕` to remove it if needed.
* **Autosave**: Work is automatically saved to browser memory so nothing is lost if the page reloads.
* **Responsive layout**: Works on phones, tablets, and desktops.

### 3. Submission Feedback

* After submitting, a **green success box** appears at the top, indicating the data has been saved.
* The message **auto-fades after 10 seconds** or can be clicked to dismiss.

### 4. Data Storage

* All submissions go directly to a **Google Sheet**.

* Each row in the app is appended as a row in the sheet, with clearly labeled columns for:

  ```
  Row Type | Date | Form Completed By | Submission Timestamp | Total Sales | Bins Generated | Bins Distributed | Product Name | Quantity | Staff Name | Task | Start Time | End Time | Hours Worked
  ```

* **Hours Worked** is automatically calculated from start and end times and saved as a numeric value.

---

## How It Works

### Front End (Browser)

1. Built with **React**: Components manage state (rows of data, input values, etc.).
2. Built with **Tailwind CSS**: For fast, responsive, modern styling.
3. **Rows are dynamic**: Users can add or remove PRODUCT or STAFF rows, fill in data, and the app updates immediately.
4. **Autosave**: The app saves input data to the browser’s `localStorage` automatically every time a row changes.
5. **Submission**: When the “Save Report” button is clicked, all rows are sent to the Google Apps Script backend.

### Backend (Google Apps Script)

1. The backend is hosted as a **Google Apps Script Web App** connected to a Google Sheet.
2. **doPost() function** receives data from the app.
3. It loops over all rows and appends each to the Google Sheet.
4. If a row is a STAFF row with start and end times, the script calculates the hours worked as a numeric value.
5. Returns a **success/failure response** to the front end.

---

## How to Use

1. **Initial setup**: On first load, enter the **Google Apps Script Web App URL** and click “Save & Continue.”
2. **Enter daily data**: The top DAY row is prefilled with today’s date. Fill in total sales, bins, and who completed the form.
3. **Add products**: Click `+ Add Product` to add rows for each product sold, and enter quantity.
4. **Add staff**: Click `+ Add Staff` to add staff rows. Enter staff name, task, and start/end times. Hours worked are calculated automatically.
5. **Remove rows**: Click the `✕` on any row to remove it.
6. **Submit**: Click “Save Report.” A green confirmation will appear, and the data will be sent to Google Sheets.

---

## Technical details

* **Front End**:

  * React manages state via `useState` and `useEffect`.
  * Rows are stored as an array of objects. Each object has keys corresponding to row data.
  * Autosave leverages `localStorage` for resilience.
  * Form submission builds a `FormData` object with all rows and sends a POST request to the Web App URL.

* **Backend**:

  * Apps Script reads `e.parameters` to retrieve all form fields (handles repeated keys).

  * Appends each row to the Google Sheet using `appendRow()`.

  * STAFF hours are calculated in JavaScript as:

    ```javascript
    hoursWorked = (endTime - startTime) in milliseconds / 3600000
    ```

  * Returns JSON `{ success: true }` on success, or `{ success: false, error: "message" }` on failure.

* **Google Sheet Layout**: Columns correspond to the headers listed above. This structure allows **easy filtering, pivot tables, and calculations** directly in Sheets.

---


## Notes

* Only **Date is required**; other fields can be left empty.
* Data persists locally in the browser until submitted.
* Works best in **modern browsers** (Chrome, Edge, Firefox, Safari).
