<script setup>
    import { reactive } from 'vue';
    const data = reactive({
        'invoiceID': '0',
        'date': 'mm/dd/yyyy',
        'address': 'Uttara, Dhaka-1230',
        'phone': '(800) 555-1234',
        'items': [
            {
                'Product': '',
                'Description': '',
                'Rate': '0',
                'Quantity': '0',
                'Discount': '0',
                'Price': '$0'
            }
        ],
        'subtotal': {
            'default': '0'
        },
        'globalDiscount': '0',
        'tax': '0',
        'balanceDue': '',
    })
    function subTotal(){
        let subtotal = 0
        data.items.forEach(item => {
            subtotal += item.Price
        })
        data.subtotal = subtotal
        return subtotal
    }

    function addMoreRow(){
        data.items.push({
            'Product': '',
            'Description': '',
            'Rate': '0',
            'Quantity': '0',
            'Discount': '0',
            'Price': '0'
        })
    }

    function removeRow() {
        data.items.splice(0, 1);
    }

    function balanceDue(){
        const tax = data.subtotal * data.tax / 100
        data.balanceDue = data.subtotal + tax - data.globalDiscount
        return data.balanceDue
    }

</script>
<template>
    <div class="container">
        <header>
                <h1>Recipient</h1>
                <address contenteditable>
                    <p>Some Company<br>c/o Some Guy</p>
                </address>
                <span><img alt="" src="http://www.jonathantneal.com/examples/invoice/logo.png"><input type="file" accept="image/*"></span>
        </header>
        <article>
            <h1>Invoice</h1>
            <table class="meta1">
                <tr>
                    <th><span contenteditable>Address</span></th>
                    <td><input type="text" contenteditable v-model="data.address"></td>
                </tr>
                <tr>
                    <th><span contenteditable>Phone</span></th>
                    <td><input type="text" contenteditable v-model="data.phone"></td>
                </tr>
            </table>
            <table class="meta">
                <tr>
                    <th><span contenteditable>Invoice #</span></th>
                    <td><input type="number" contenteditable v-model="data.invoiceID"></td>
                </tr>
                <tr>
                    <th><span contenteditable>Date</span></th>
                    <td><input type="text" contenteditable v-model="data.date"></td>
                </tr>
                <!-- <tr>
                    <th><span contenteditable>Amount Due</span></th>
                    <td><input id="prefix" contenteditable value="$600.00"></td>
                </tr> -->
            </table>
            <table class="inventory">
                <thead>
                    <tr>
                        <th><span contenteditable>Product</span></th>
                        <th><span contenteditable>Description</span></th>
                        <th><span contenteditable>Rate</span></th>
                        <th><span contenteditable>Quantity</span></th>
                        <th><span contenteditable>Discount ($)</span></th>
                        <th><span contenteditable>Price</span></th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(item, index) in data.items" :key="index">
                        <td><a class="cut" @click="removeRow()">-</a><input type="text" contenteditable v-model="item.Product"></td>
                        <td><input type="text" contenteditable v-model="item.Description"></td>
                        <td><input class="text-end" type="text" contenteditable v-model="item.Rate"></td>
                        <td><input class="text-end" type="text" contenteditable v-model="item.Quantity"></td>
                        <td><input class="text-end" type="text" contenteditable v-model="item.Discount"></td>
                        <td class="text-end">${{ item.Discount ? item.Price=item.Rate*item.Quantity-item.Discount : item.Price=item.Rate*item.Quantity }}</td>
                    </tr>
                </tbody>
            </table>
            <a class="add" @click="addMoreRow()">+</a>
            <table class="balance">
                <tr>
                    <th><span contenteditable>Sub Total</span></th>
                    <td><span data-prefix>$</span><span>{{ subTotal() }}</span></td>
                </tr>
                <!-- <tr>
                    <th><span contenteditable>Amount Paid</span></th>
                    <td><input type="number" contenteditable v-model="data.tax"></td>
                </tr> -->
                <tr>
                    <th><span contenteditable>Global Discount ($)</span></th>
                    <td><input class="text-end" type="text" contenteditable v-model="data.globalDiscount"></td>
                </tr>
                <tr>
                    <th><span contenteditable>Tax</span></th>
                    <td><input class="text-end" type="text" contenteditable v-model="data.tax"></td>
                </tr>
                <tr>
                    <th><span contenteditable>Balance Due</span></th>
                    <td><span data-prefix>$</span><span>{{ balanceDue() }}</span></td>
                </tr>
            </table>
        </article>
        <aside>
            <h1><span contenteditable>Additional Notes</span></h1>
            <div contenteditable>
                <p>A finance charge of 1.5% will be made on unpaid balances after 30 days.</p>
            </div>
        </aside>
    </div>
</template>

<style scoped>

    input {
        background-color: #FFF;
    }

    *
    {
        border: 0;
        box-sizing: content-box;
        color: inherit;
        font-family: inherit;
        font-size: inherit;
        font-style: inherit;
        font-weight: inherit;
        line-height: inherit;
        list-style: none;
        margin: 0;
        padding: 0;
        text-decoration: none;
        vertical-align: top;
    }

    /* content editable */

    *[contenteditable] { border-radius: 0.25em; min-width: 1em; outline: 0; }

    *[contenteditable] { cursor: pointer; }

    *[contenteditable]:hover, *[contenteditable]:focus, td:hover *[contenteditable], td:focus *[contenteditable], img.hover { background: #DEF; box-shadow: 0 0 1em 0.5em #DEF; }

    span[contenteditable] { display: inline-block; }

    /* heading */

    h1 { font: bold 100% sans-serif; letter-spacing: 0.5em; text-align: center; text-transform: uppercase; }

    /* table */

    table { font-size: 75%; table-layout: fixed; width: 100%; }
    table { border-collapse: separate; border-spacing: 2px; }
    th, td { border-width: 1px; padding: 0.5em; position: relative; text-align: left; }
    th, td { border-radius: 0.25em; border-style: solid; }
    th { background: #EEE; border-color: #BBB; }
    td { border-color: #DDD; }

    /* page */

    html { font: 16px/1 'Open Sans', sans-serif; overflow: auto; padding: 0.5in; }
    html { background: #999; cursor: default; }

    body { box-sizing: border-box; height: 11in; margin: 0 auto; overflow: hidden; padding: 0.5in; width: 8.5in; }
    body { background: #FFF; border-radius: 1px; box-shadow: 0 0 1in -0.25in rgba(0, 0, 0, 0.5); }

    /* header */

    header { margin: 0 0 3em; }
    header:after { clear: both; content: ""; display: table; }

    header h1 { background: #000; border-radius: 0.25em; color: #FFF; margin: 0 0 1em; padding: 0.5em 0; }
    header address { float: left; font-size: 75%; font-style: normal; line-height: 1.25; margin: 0 1em 1em 0; }
    header address p { margin: 0 0 0.25em; }
    header span, header img { display: block; float: right; }
    header span { margin: 0 0 1em 1em; max-height: 25%; max-width: 60%; position: relative; }
    header img { max-height: 100%; max-width: 100%; }
    header input { cursor: pointer; -ms-filter:"progid:DXImageTransform.Microsoft.Alpha(Opacity=0)"; height: 100%; left: 0; opacity: 0; position: absolute; top: 0; width: 100%; }

    /* article */

    article, article address, table.meta, table.inventory { margin: 0 0 3em; }
    article:after { clear: both; content: ""; display: table; }
    article h1 { clip: rect(0 0 0 0); position: absolute; }

    article address { float: left; font-size: 125%; font-weight: bold; }

    /* table meta & balance */

    table.meta, table.balance { float: right; width: 36%; }
    table.meta:after, table.balance:after { clear: both; content: ""; display: table; }

    /* table meta */

    table.meta th { width: 40%; }
    table.meta td { width: 60%; }
    article, article address, table.meta1, table.inventory { margin: 0 0 3em; }
    article:after { clear: both; content: ""; display: table; }
    article h1 { clip: rect(0 0 0 0); position: absolute; }

    article address { float: left; font-size: 125%; font-weight: bold; }

    /* table meta & balance */

    table.meta1 { float: left; width: 36%; }

    /* table meta */

    table.meta1 th { width: 40%; }
    table.meta1 td { width: 60%; }

    /* table items */

    table.inventory { clear: both; width: 100%; }
    table.inventory th { font-weight: bold; text-align: center; }

    table.inventory td:nth-child(1) { width: 26%; }
    table.inventory td:nth-child(2) { width: 38%; }
    table.inventory td:nth-child(3) { text-align: right; width: 12%; }
    table.inventory td:nth-child(4) { text-align: right; width: 12%; }
    table.inventory td:nth-child(5) { text-align: right; width: 12%; }

    /* table balance */

    table.balance th, table.balance td { width: 50%; }
    table.balance td { text-align: right; }

    /* aside */

    aside h1 { border: none; border-width: 0 0 1px; margin: 0 0 1em; }
    aside h1 { border-color: #999; border-bottom-style: solid; }

    /* javascript */

    .add, .cut
    {
        border-width: 1px;
        display: block;
        font-size: .8rem;
        padding: 0.25em 0.5em;	
        float: left;
        text-align: center;
        width: 0.6em;
    }

    .add, .cut
    {
        background: #9AF;
        box-shadow: 0 1px 2px rgba(0,0,0,0.2);
        background-image: -moz-linear-gradient(#00ADEE 5%, #0078A5 100%);
        background-image: -webkit-linear-gradient(#00ADEE 5%, #0078A5 100%);
        border-radius: 0.5em;
        border-color: #0076A3;
        color: #FFF;
        cursor: pointer;
        font-weight: bold;
        text-shadow: 0 -1px 2px rgba(0,0,0,0.333);
    }

    .add { margin: -2.5em 0 0; }

    .add:hover { background: #00ADEE; }

    .cut { opacity: 0; position: absolute; top: 0; left: -1.5em; }
    .cut { -webkit-transition: opacity 100ms ease-in; }

    tr:hover .cut { opacity: 1; }

    @media print {
        * { -webkit-print-color-adjust: exact; }
        html { background: none; padding: 0; }
        body { box-shadow: none; margin: 0; }
        span:empty { display: none; }
        .add, .cut { display: none; }
    }

    @page { margin: 0; }
</style>