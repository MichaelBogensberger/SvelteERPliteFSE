<script>
  import Router from "svelte-spa-router";

  import Home from "./routes/Home.svelte";
  import Customers from "./routes/Customers.svelte";
  import NotFound from "./routes/NotFound.svelte";

  import "carbon-components-svelte/css/g100.css";
  import { Button } from "carbon-components-svelte";
  import {location} from 'svelte-spa-router'

  import {
    Header,
    HeaderNav,
    HeaderNavItem,
    HeaderNavMenu,
    SkipToContent,
    Content,
    Grid,
    Row,
    Column,
  } from "carbon-components-svelte";
  import { ClickableTile } from "carbon-components-svelte";

  let isSideNavOpen = false;

  let routes = {
    "/": Home,
    "/customers": Customers,
    "*": NotFound,
  };

 
  
</script>

<main>
  <Header platformName="ErpLite Michael Bogensberger" bind:isSideNavOpen>
    <svelte:fragment slot="skip-to-content">
      <SkipToContent />
    </svelte:fragment>
    <HeaderNav>
		<!-- isSelected -->
      <HeaderNavItem  href="/" text="Home" />
      <HeaderNavItem href="/#/customers" text="Customers" />
      <HeaderNavItem href="/" text="Link 3" />
      <HeaderNavMenu text="Menu">
        <HeaderNavItem href="/" text="Link 1" />
        <HeaderNavItem href="/" text="Link 2" />
        <HeaderNavItem href="/" text="Link 3" />
      </HeaderNavMenu>
    </HeaderNav>
  </Header>


  <style>
    h1 {
      color: #ff3e00;
      font-size: 4em;
      font-weight: 400;
    }
  </style>

  <Router {routes} />
</main>
