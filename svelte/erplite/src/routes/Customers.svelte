<script>
  import { DataTable } from "carbon-components-svelte";
  import { Grid, Row, Column } from "carbon-components-svelte";
  import { ClickableTile } from "carbon-components-svelte";


  import {
    Button,
    ComposedModal,
    ModalHeader,
    ModalBody,
    ModalFooter,
    Checkbox,
  } from "carbon-components-svelte";

  import {
    FluidForm,
    TextInput,
    PasswordInput,
  } from "carbon-components-svelte";


  let open = false;
  let checked = false;


</script>

<Grid fullWidth>
  <Row>
    <Column lg={4} />
    <Column lg={8}>
      <br /><br /><br /><br /><br /><br />

      <DataTable
        title="Customers"
        description="Here you can see all Customers"
        headers={[
          { key: "firstname", value: "Firstname" },
          { key: "lastname", value: "Lastname" },
          { key: "email", value: "Email" },
        ]}
        rows={[
          {
            id: "a",
            name: "Load Balancer 3",
            protocol: "HTTP",
            port: 3000,
          },
          {
            id: "b",
            name: "Load Balancer 1",
            protocol: "HTTP",
            port: 443,
          },
        ]}
      />
    </Column>
    <Column lg={4} />
  </Row>

  <br /><br />

  <Row>
    <Column lg={4} />

    <Column>
      <ClickableTile on:click={() => (open = true)}>add Customer</ClickableTile>
    </Column>

    <Column lg={4} />
  </Row>
</Grid>



<ComposedModal bind:open on:submit={() => (open = false)}>
  <ModalHeader label="Customer" title="add Customer" />
  <ModalBody hasForm>

    <TextInput labelText="Firstname" placeholder="Max" required />
    <TextInput labelText="Lastname" placeholder="Mustermann" required />
    <TextInput labelText="Email" type="email" placeholder="max@gmx.at" required />
    <br>
    <TextInput inline labelText="Street" placeholder="Prausgasse 12" />
    <TextInput inline labelText="Zipcode" placeholder="7213" type="number" />
    <TextInput inline labelText="City" placeholder="Telfs" />
    <TextInput inline labelText="Country" placeholder="Österreich" />

    
  </ModalBody>

  <ModalFooter primaryButtonText="Proceed" type="submit"  />

</ComposedModal>




<style>
  h1 {
    color: #ff3e00;
    font-size: 4em;
    font-weight: 400;
  }
</style>
