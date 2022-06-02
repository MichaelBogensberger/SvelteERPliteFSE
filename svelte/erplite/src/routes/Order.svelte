<script>
  import {
    DataTable,
    Toolbar,
    ToolbarContent,
    ToolbarSearch,
    ToolbarMenu,
    ToolbarMenuItem,
    ToolbarBatchActions,
    Button,
    ComposedModal,
    ModalHeader,
    ModalBody,
    ModalFooter,
    Form,
  } from "carbon-components-svelte";
  import { Grid, Row, Column } from "carbon-components-svelte";
  import { ClickableTile } from "carbon-components-svelte";
  import { onMount } from "svelte";
  import { Loading } from "carbon-components-svelte";
  import { TextArea } from "carbon-components-svelte";
  import { MultiSelect } from "carbon-components-svelte";

  import {
    FluidForm,
    TextInput,
    PasswordInput,
  } from "carbon-components-svelte";

  const headers = [
    { key: "orderID", value: "OrderID" },
    { key: "customerID", value: "CustomerID" },
    { key: "customerFirstname", value: "Firstname" },
    { key: "customerLastname", value: " Lastname" },
    { key: "customerEmail", value: "Email" },
    { key: "grossTotal", value: "Total" },
    { key: "state", value: "State" },
  ];

  let rows = [];
  let orders = [];
  let table_orders = [];
  const url = "http://localhost:8080";

  let openOrderModal = false;
  let active = false;

  let openVerifyPaymentModal = false;

  onMount(async () => {
    getAllOrdersForTable();
  });

  async function getAllOrdersForTable() {
    active = true;
    const response = await fetch(url + "/api/v1/orders/");
    orders = await response.json();
    //console.log(orders)

    table_orders = orders;

    table_orders.forEach((element) => {
      [
        "orderLineItems",
        "taxTotal",
        "customerStreet",
        "customerZipcode",
        "date",
        "netTotal",
        "customerCity",
        "customerCountry",
      ].forEach((e) => delete element[e]);
      element.id = Math.random();
    });

    //console.log(table_orders)
    rows = [];
    console.log(rows);
    rows = rows.concat(table_orders);
    active = false;
    //console.log(rows)
  }

  let verifyOrderModalID;

  /*
  var item = {
    productNumber: "1234567891",
    productName: "IPhone",
    priceNet: 100,
    tax: 10,
    amount: 110,
  };
  */

  let customerID = generate(10);
  let customerFirstname;
  let customerLastname;
  let customerEmail;
  let customerStreet;
  let customerZipcode;
  let customerCity;
  let customerCountry;
  //let cartItemsBind = JSON.stringify(item);
  //let cartItems = [];
  //cartItems.push(JSON.parse(cartItemsBind));

  //console.log("cartItemsBind:");
  //console.log(cartItemsBind);

  //console.log("cartItems");
  //console.log(cartItems);

  let products = [
    {
      id: 0,
      productNumber: "6754328192",
      productName: "Pulp Fiction",
      priceNet: 20,
      tax: 5,
      amount: 25,
    },

    {
      id: 1,
      productNumber: "6754328192",
      productName: "Goodfellas",
      priceNet: 20,
      tax: 5,
      amount: 25,
    },

    {
      id: 2,
      productNumber: "6754328192",
      productName: "Der Pate 2",
      priceNet: 20,
      tax: 5,
      amount: 25,
    },
  ];

  let cartItems = [];

  const items = [
    { id: "0", text: "Pulp Fiction" },
    { id: "1", text: "Goodfellas" },
    { id: "2", text: "Der Pate 2" },
  ];
  let multiselect_selectedIds = [];

  async function handleSubmit() {
    openOrderModal = false;

    for (const id of multiselect_selectedIds) {
      for (const product of products) {
        if (product.id == id) {
          cartItems.push(product);
        }
        //console.log(product.id);
      }
    }

    console.log(cartItems);

    placeOrder();
    //console.log(multiselect_selectedIds);
  }

  async function placeOrder() {
    const res = await fetch(url + "/api/v1/orders/", {
      method: "POST",
      body: JSON.stringify({
        customerID,
        customerFirstname,
        customerLastname,
        customerEmail,
        customerStreet,
        customerZipcode,
        customerCity,
        customerCountry,
        cartItems,
      }),
      headers: {
        "content-type": "application/json",
      },
    }).then(function () {
      getAllOrdersForTable();
      customerID = generate(10)
    });
    /*
		const json = await res.json()
		result = JSON.stringify(json)
    console.log(result)
    */
  }

  async function handleVerifyPayment() {
    openVerifyPaymentModal = false;
    setPaymentToVerified();
  }

  async function setPaymentToVerified() {
    const res = await fetch(
      url + "/api/v1/orders/checkpayment/" + verifyOrderModalID,
      {
        method: "POST",
        headers: {
          "content-type": "application/json",
        },
      }
    ).then(function () {
      getAllOrdersForTable();
    });
  }

  function generate(n) {
    var add = 1,
      max = 12 - add; // 12 is the min safe number Math.random() can generate without it starting to pad the end with zeros.

    if (n > max) {
      return generate(max) + generate(n - max);
    }

    max = Math.pow(10, n + add);
    var min = max / 10; // Math.pow(10, n) basically
    var number = Math.floor(Math.random() * (max - min + 1)) + min;

    return ("" + number).substring(add);
  }


</script>

<br /><br /><br /><br /><br /><br /><br /><br />

<Loading {active} />

<Grid fullWidth>
  <Row>
    <Column lg={4} />
    <Column lg={8}>
      <DataTable
        title="Orders"
        description="Hier kannst du Orders einsehen"
        {headers}
        {rows}
      />
    </Column>
    <Column lg={4} />
  </Row>

  <br /><br />
  <Row>
    <Column lg={4} />

    <Column>
      <ClickableTile on:click={() => (openOrderModal = true)}
        >place Order</ClickableTile
      >
    </Column>

    <Column>
      <ClickableTile on:click={() => getAllOrdersForTable()}
        >reload Table</ClickableTile
      >
    </Column>

    <Column lg={4} />
  </Row>

  <br />

  <Row>
    <Column lg={4} />
    <Column>
      <ClickableTile on:click={() => (openVerifyPaymentModal = true)}
        >verfiy payment</ClickableTile
      >
    </Column>

    <Column lg={4} />
  </Row>
</Grid>

{#if openOrderModal}
  <ComposedModal
    open
    on:submit={() => handleSubmit()}
    preventCloseOnClickOutside
  >
    <ModalHeader label="Order" title="place Order" />
    <ModalBody hasForm>
      <MultiSelect
        bind:selectedIds={multiselect_selectedIds}
        {items}
        titleText="Product"
        label="Select product to order..."
      />

      <TextInput
        bind:value={customerID}
        labelText="customerID"
        placeholder="1234567891"
        required
      />
      <TextInput
        bind:value={customerFirstname}
        labelText="customerFirstname"
        placeholder="Max"
        required
      />
      <TextInput
        bind:value={customerLastname}
        labelText="customerLastname"
        placeholder="Mustermann"
        required
      />
      <TextInput
        bind:value={customerEmail}
        labelText="customerEmail"
        type="email"
        placeholder="max@gmx.at"
        required
      />
      <TextInput
        bind:value={customerStreet}
        labelText="customerStreet"
        placeholder="Sesamstraße 2"
        required
      />
      <TextInput
        bind:value={customerZipcode}
        labelText="customerZipcode"
        placeholder="6460"
        required
      />
      <TextInput
        bind:value={customerCity}
        labelText="customerCity"
        placeholder="Imst"
        required
      />
      <TextInput
        bind:value={customerCountry}
        labelText="customerCountry"
        placeholder="Österreich"
        required
      />
      <br />
      <!--
    <p>Cart Item</p>
    <TextInput inline labelText="productNumber" placeholder="1234567891" />
    <TextInput inline labelText="productName" placeholder="IPhone 6" type="number" />
    <TextInput inline labelText="priceNet" placeholder="100" />
    <TextInput inline labelText="tax" placeholder="20" />
    <TextInput inline labelText="amount" placeholder="120" />
    -->

      <!--
    <TextArea

      rows={20}
      labelText="App CartItems"
      bind:value={cartItemsBind}
    />

    -->
    </ModalBody>
    <ModalFooter
      primaryButtonText="Proceed"
      type="submit"
      secondaryButtons={[{ text: "Cancel" }]}
      on:click:button--secondary={({ detail }) => {
        if (detail.text === "Cancel") openOrderModal = false;
      }}
    />
  </ComposedModal>
{/if}

{#if openVerifyPaymentModal}
  <ComposedModal
    open
    on:submit={() => handleVerifyPayment()}
    preventCloseOnClickOutside
  >
    <ModalHeader label="Order" title="set order status to payed (verified)" />
    <ModalBody hasForm>
      <TextInput
        bind:value={verifyOrderModalID}
        inline
        labelText="Oder Number"
        placeholder="O323567890"
      />
    </ModalBody>
    <ModalFooter
      primaryButtonText="Proceed"
      type="submit"
      secondaryButtons={[{ text: "Cancel" }]}
      on:click:button--secondary={({ detail }) => {
        if (detail.text === "Cancel") openVerifyPaymentModal = false;
      }}
    />
  </ComposedModal>
{/if}

<style>
  h1 {
    color: #ff3e00;
    font-size: 4em;
    font-weight: 400;
  }
</style>
