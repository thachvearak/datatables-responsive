#datatables-responsive

##Introduction
[Datatables][1] is hands down the best jQuery table plugin.  I have enjoyed using it over the years and highly recommend it to all.  Recently, I have tried to use Datatables in an responsive web project.  Datatables did everything brilliantly but was not responsive.  After some research, I found [FooTable][2] which handle the responsive behavior perfectly.  After tinkering around, I've come up with something that helps make Datatables respond like FooTable.

Below are the instructions to use the helper.

##Create variables and break point definitions.

```javascript
var responsiveHelper;
var breakpointDefinition = {
    tablet: 1024,
    phone : 480
};
var tableContainer = $('#example');
```


##Create Datatables Instance
Create the datatables instance with the following

- Set bAutoWidth: false
- Set fnPreDrawCallback to only initialize the responsive datatables helper once
- Set fnRowCallback to create expand icon.
- Set fnDrawCallback to respond to window resize events.

```javascript
tableContainer.dataTable({

    // Setup for Bootstrap support.
    sDom           : '<"row"<"span6"l><"span6"f>r>t<"row"<"span6"i><"span6"p>>',
    sPaginationType: 'bootstrap',
    oLanguage      : {
        sLengthMenu: '_MENU_ records per page'
    },

    // Setup for responsive datatables helper.
    bAutoWidth     : false,
    fnPreDrawCallback: function () {
        // Initialize the responsive datatables helper once.
        if (!responsiveHelper) {
            responsiveHelper = new responsiveDatatablesHelper(tableContainer, breakpointDefinition);
        }
    },
    fnRowCallback  : function (nRow, aData, iDisplayIndex, iDisplayIndexFull) {
        responsiveHelper.createExpandIcon(nRow);
    },
    fnDrawCallback : function (oSettings) {
        responsiveHelper.respond();
    }

});
```

## Add Data Attributes to the Table Elements
5. Add the data-class="expand" attribute to the th element for the respective column that will you want to display the expand icon in.  This th elment cannot be a column that will be hidden.

6. Add data-hide="phone,tablet" to the th element for the respective column that will you want to hide when the window is resized.

```html
<div class="span12">
    <table id="example" cellpadding="0" cellspacing="0" border="0" class="table table-bordered table-striped">
        <!--thead section is required-->
        <thead>
        <tr>
            <th class="centered-cell"><input type="checkbox" id="masterCheck" class="checkbox"/></th>
            <th data-class="expand">Rendering engine</th>
            <th>Browser</th>
            <th data-hide="phone">Platform(s)</th>
            <th data-hide="phone,tablet">Engine version</th>
            <th data-hide="phone,tablet">CSS grade</th>
        </tr>
        </thead>
        <!--tbody section is required-->
        <tbody></tbody>
        <!--tfoot section is optional-->
        <tfoot>
        <tr>
            <th></th>
            <th>Engine</th>
            <th>Browser</th>
            <th>Platform(s)</th>
            <th>Engine version</th>
            <th>CSS grade</th>
        </tr>
        </tfoot>
    </table>

</div>
```

That's it!

##Thanks
Thanks to Allan Jardine for making the best data table plugin for jQuery.  Nothing out there comes close.

I would also like to thank thanks Brad Vincent and his friend Steve for making the awesome responsive FooTable ([https://github.com/bradvin/FooTable][3]).  In my opinion, their implementation for a responsive table is the best to date.  Much of what I have done here is borrowed from FooTable.  Thanks again!


  [1]: http://datatables.net/
  [2]: http://themergency.com/footable/
  [3]: https://github.com/bradvin/FooTable
