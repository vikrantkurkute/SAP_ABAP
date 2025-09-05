# ZSELECTION_SCREEN - ABAP Demo Program

## 🎯 Purpose of the Program
The **ZSELECTION_SCREEN** demo report is designed to showcase and explain the usage of various **Selection Screen elements** and **ABAP report events** in a single, easy-to-understand program.  

It serves as a reference template for **ABAP developers** and **functional consultants** to:
- Understand how to implement and control **SELECT-OPTIONS**, **PARAMETERS**, **CHECKBOXES**, **RADIOBUTTONS**, **DROPDOWNS**, and **PUSHBUTTONS**.
- Learn how to use event blocks such as `INITIALIZATION`, `AT SELECTION-SCREEN`, `AT SELECTION-SCREEN OUTPUT`, `START-OF-SELECTION`, and `AT USER-COMMAND`.
- See how to dynamically modify screen fields, populate dropdown values at runtime, handle user actions, and insert validations.
- Help teams accelerate development by reusing or adapting this code for actual business reports or tools.

---

## ✅ Use Cases
- As a **learning tool** for new ABAP developers.
- As a **ready reference** to copy-paste commonly used UI patterns.
- To demonstrate **user interaction handling** in classical SAP reports.
- To help **functional consultants** visualize selection screen possibilities during requirements gathering.

---

## 📝 Notes for Developers
- **Modular Design:** Easy to plug and play with real values and logic.
- **Breakpoints:** Added intentionally to demonstrate execution sequence.
- **Comments:** Make it beginner-friendly.

### Best Practices:
- Use **screen groups** to control visibility/input dynamically.
- Use **VRM_SET_VALUES** for dropdown flexibility.
- Always validate required inputs in `AT SELECTION-SCREEN`.

---

## 🧪 Testing Points (With Breakpoints)
- **Selection screen load** (`INITIALIZATION`)
- **After user presses Enter** (`AT SELECTION-SCREEN`)
- **Dropdown population** (`AT SELECTION-SCREEN OUTPUT`)
- **Clicks on push buttons** (`AT USER-COMMAND`)
- **Click on ALV/output lines** (`AT LINE-SELECTION`)

## 📜 Line-by-Line / Section-by-Section Explanation

### **Report Declaration**
```abap
REPORT zselection_screen.                        "Declares the name of the ABAP report

DATA : temp_matnr TYPE mara-matnr.              "Temporary material number variable for use in input fields

SELECTION-SCREEN BEGIN OF BLOCK b1 WITH FRAME TITLE TEXT-001. "Starts a titled block on the selection screen

SELECT-OPTIONS : s_matnr1 FOR temp_matnr MODIF ID DIS. "Select-option for material with modification group for dynamic control
SELECT-OPTIONS : s_matnr2 FOR temp_matnr OBLIGATORY DEFAULT '001' NO INTERVALS NO-EXTENSION. "Mandatory, single-value field with default value

PARAMETERS : R1 RADIOBUTTON GROUP grp1 USER-COMMAND R_UCOMM, "First radio button in a group, triggers user command
             R2 RADIOBUTTON GROUP grp1 MODIF ID hid.          "Second radio button in the same group

PARAMETERS : check AS CHECKBOX DEFAULT abap_true. "A checkbox checked by default
PARAMETERS : p_matnr TYPE matnr. "Simple parameter input for material number
PARAMETERS DROP_D TYPE mara-matnr AS LISTBOX VISIBLE LENGTH 18. "Dropdown listbox parameter filled dynamically at runtime

SELECTION-SCREEN PUSHBUTTON 52(10) T  USER-COMMAND BUT1. "First push button with dynamic text
SELECTION-SCREEN PUSHBUTTON /52(10) text-002  USER-COMMAND BUT2. "Second push button using text symbol

SELECTION-SCREEN END OF BLOCK b1. "Ends the selection screen block

INITIALIZATION. "Triggered once before the screen is displayed
T = 'BUTTON 1'. "Sets the display text for the first push button
IF R1 IS INITIAL AND R2 IS INITIAL.
  R1 = ABAP_TRUE. "Ensures R1 is selected by default
ENDIF.
AT SELECTION-SCREEN. "General validation when user presses ENTER
AT SELECTION-SCREEN ON check.
BREAK-POINT.
IF R1 EQ ABAP_TRUE.
  MESSAGE 'ERROR' TYPE 'I'.
ENDIF.

AT SELECTION-SCREEN OUTPUT. "Used to modify screen and populate dropdown
DATA: vrm_id TYPE vrm_id, vrm_values TYPE vrm_values, vrm_value LIKE LINE OF vrm_values.

vrm_value-key = 'Y'. vrm_value-text = 'YES'. APPEND vrm_value TO vrm_values.
vrm_value-key = 'N'. vrm_value-text = 'NO'. APPEND vrm_value TO vrm_values.

vrm_id = 'DROP_D'.
CALL FUNCTION 'VRM_SET_VALUES'
  EXPORTING id = 'DROP_D' values = vrm_values.

IF sy-subrc <> 0.
  MESSAGE ID sy-msgid TYPE sy-msgty NUMBER sy-msgno
    WITH sy-msgv1 sy-msgv2 sy-msgv3 sy-msgv4.
ENDIF.

LOOP AT SCREEN.
  IF SCREEN-group1 EQ 'DIS'.
    SCREEN-input = 0.
    MODIFY SCREEN.
  ENDIF.
ENDLOOP.

"F4 Help (Value Request)
AT SELECTION-SCREEN ON VALUE-REQUEST FOR p_matnr. 
AT SELECTION-SCREEN ON VALUE-REQUEST FOR s_matnr1-LOW.
AT SELECTION-SCREEN ON VALUE-REQUEST FOR s_matnr1-HIGH.

START-OF-SELECTION.
DATA(HELLO) = |HELLO|.
WRITE :/ HELLO.

AT LINE-SELECTION. => Triggered when user clicks on an output line (interactive report).
DATA : GV_FNAME(30), GV_VALUE(30).
GET CURSOR FIELD gv_fname VALUE gv_value.
BREAK-POINT.

AT USER-COMMAND. "=> Handles user-defined commands like push buttons.
CASE sy-ucomm. => Triggered when user clicks on an output line (interactive report).
  BREAK-POINT.
  WHEN '&EXIT'.
ENDCASE.




