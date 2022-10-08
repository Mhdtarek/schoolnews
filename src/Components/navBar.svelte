<script>
  import {
    Navbar,
    NavbarBrand,
    Nav,
    NavItem,
    NavLink,
    Dropdown,
    DropdownItem,
    DropdownMenu,
    DropdownToggle,
  } from "sveltestrap";
  import {
    userPhotoURL,
    userDisplayName,
    userCreation,
    isLoggedIn,
  } from "./Auth.svelte";
</script>

<Navbar color="light" light expand="md">
  <NavbarBrand color="light" href="#">skola nyheter</NavbarBrand>
  <Nav class="ms-auto" navbar>
    {#if $isLoggedIn}
      <NavItem>
        <Dropdown direction="left" nav setActiveFromChild>
          <DropdownToggle nav class="nav-link"
            ><img
              style="border-radius: .25rem;"
              src={$userPhotoURL}
              alt="user"
              height="32"
              width="32"
            /></DropdownToggle
          >
          <DropdownMenu dark>
            <DropdownItem header>{$userDisplayName}</DropdownItem>
            <DropdownItem href="#">inställningar</DropdownItem>
            <DropdownItem
              color="primary"
              on:click={() => window.location.reload()}>Logga ut</DropdownItem
            >
          </DropdownMenu>
        </Dropdown>
      </NavItem>
    {/if}
    <NavItem />
  </Nav>
</Navbar>
