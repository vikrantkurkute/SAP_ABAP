# 🚀 ABAP ALV Factory Method Report - `ZZVIK_ALV_FACTORY`

## 📌 Purpose

This report demonstrates how to build an **ALV (ABAP List Viewer) report using the SALV Factory Method**.  
It provides a reusable template for SAP ABAP developers to quickly create interactive ALV reports with:

- Selection screens for user input  
- Optimized column layout and display  
- Custom headers and zebra striping  
- Standard ALV toolbar functions (Export, Sort, Filter, Find)  
- Exception handling and extensibility  

---

## ✅ Key Features

- **Factory Method Usage**  
  Utilizes `cl_salv_table=>factory` to instantiate ALV objects.

- **Dynamic Column Customization**  
  - Set short/medium/long texts  
  - Optimize column widths  
  - Define output length  

- **Enhanced Display Settings**  
  - Zebra pattern for readability  
  - Custom report header  

- **Selection Screen Support**  
  - Filter by `MATNR` (Material Number)

- **Standard ALV Functions Enabled**  
  - Export to Excel  
  - Sort, Filter, Find  

- **Robust Error Handling**  
  - Uses `cx_salv_msg` for exception management  

---

## 🛠️ Technical Details

| Attribute       | Value                  |
|----------------|------------------------|
| **Report Name** | `ZZVIK_ALV_FACTORY`    |
| **Primary Table** | `MARA` (Material Master) |

---

## 🧩 Implementation Notes

To add custom buttons or enhance toolbar functionality, use the following:

```abap
lo_alv->set_screen_status(
  report        = sy-repid,
  pfstatus      = 'STANDARD',
  set_functions = cl_salv_model_base=>c_functions_all
).

## 🖼️ Output Preview

- **Header:** ALV Test for Display Settings  
- **Layout:** Zebra pattern with optimized columns  
- **Toolbar:** Export, Sort, Filter, Find enabled  

## 📅 Change Log

| Date       | Author           | Description                      |
|------------|------------------|----------------------------------|
| 03-12-2024 | Vikrant Kurkute  | Initial version with ALV Factory |

## 🧠 Best Practices

- Always wrap ALV logic in `TRY...CATCH` blocks  
- Use column optimization for better UX  
- Define a custom PF-STATUS for additional UI controls  
- Reuse this template across similar reporting needs  


## ✅ Summary

`ZZVIK_ALV_FACTORY` is a ready-to-use **ALV report template** for SAP ABAP developers.  
It showcases best practices in:

- Factory method implementation  
- Dynamic UI customization  
- Interactive ALV functionality  
