# Toggle drawings used as buttons in Google Sheets

Sample code that shows how to use Google Sheets drawings as conventional UI buttons to invoke Apps Script functions. Code is based on this post ([Switching Buttons for Google Spreadsheet using Google Apps Script](https://tanaikech.github.io/2020/07/27/switching-buttons-for-google-spreadsheet-using-google-apps-script/)) by **Kanshi TANAIKE**, that demonstrates different ways of emulating the look and feel of standard buttons inside Google Sheets.

More specifically, this function _very slightly_ improves upon Kanshi's _pattern 1_, where alternate drawings are used as a single button, that nevertheless **requires changing focus** from the current sheet to a temporary one and back again for changes to visually take effect.

![Alternar botones con dibujos hdc](https://user-images.githubusercontent.com/12829262/101820807-650c4b80-3b27-11eb-8263-49975712fc03.gif)

This alternate approach hides drawings (buttons) accordingly by moving them beyond the boundaries of the current sheet. To accomplish that, **large negative offsets** are applied to them using method [setPosition()](https://developers.google.com/apps-script/reference/spreadsheet/drawing#setPosition(Integer,Integer,Integer,Integer)) of class `Drawing`. **This does not require any kind of trick to update the presentation**, apparently not even the use of `SpreadsheetApp.flush()`, unless several updates are required during a single run of the toggle function.

```javascript
 // Toggle button state by moving appropiate drawing beyond sheet bounds
  drawings.forEach(d => {
    if (d.getOnAction() == name) {
      d.setPosition(1, 1, -1000, -1000); // << Moving out of sight does not require changing focus to / from another sheet to refresh!
    }
    else {
      d.setPosition(buttonRowCol.row, buttonRowCol.col, 0, 0);
    }
  });
```

This toggle function supports several buttons in the same sheet, adjusting the rows and columns were they should be placed, as expected, and calls the assigned code for each of them.
