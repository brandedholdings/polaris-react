---
name: Frame
category: Structure
keywords:
  - navigation
  - nav
  - links
  - primary navigation
  - main navigation
  - global
  - frame
  - sidebar
  - side bar
  - loading
  - top bar
  - menu
  - toast
fullSizeExamples: true
---

# Frame

The frame component, while not visible in the user interface itself, provides the structure for any non-embedded application. It wraps the main elements and houses the primary [navigation](/components/navigation/navigation), [top bar](/components/structure/top-bar), [toast](/components/feedback-indicators/toast), and [contextual save bar](/components/forms/contextual-save-bar) components.

---

## Best practices

For the best experience when creating an application frame, use the following components:

- [Top bar](/components/structure/top-bar)
- [Navigation](/components/navigation/navigation)
- [Contextual save bar](/components/forms/contextual-save-bar)
- [Toast](/components/feedback-indicators/toast)
- [Loading](/components/feedback-indicators/loading)

---

## Examples

### Frame in the Shopify admin

Use to present the frame structure and all of its elements in the context of the Shopify admin.

```jsx
class FrameExample extends React.Component {
  state = {
    navigationVisible: false,
    isLoading: false,
    userMenuOpen: false,
    selectedItems: [],
  };

  render() {
    const {navigationVisible, isLoading, userMenuOpen} = this.state;
    const activeLocation = window.location.href;

    const resourceName = {
      singular: 'customer',
      plural: 'customers',
    };

    const items = [
      {
        id: 341,
        url: 'customers/341',
        name: 'Mae Jemison',
        location: 'Decatur, USA',
      },
      {
        id: 256,
        url: 'customers/256',
        name: 'Ellen Ochoa',
        location: 'Los Angeles, USA',
      },
    ];

    const promotedBulkActions = [
      {
        content: 'Edit customers',
        onAction: () => console.log('Todo: implement bulk edit'),
      },
    ];

    const bulkActions = [
      {
        content: 'Add tags',
        onAction: () => console.log('Todo: implement bulk add tags'),
      },
      {
        content: 'Remove tags',
        onAction: () => console.log('Todo: implement bulk remove tags'),
      },
      {
        content: 'Delete customers',
        onAction: () => console.log('Todo: implement bulk delete'),
      },
    ];

    const userMenuMarkup = (
      <TopBar.UserMenu
        actions={[
          {
            items: [
              {content: 'Your profile', icon: 'profile'},
              {content: 'Log out', icon: 'logOut'},
            ],
          },
          {
            items: [
              {content: 'Shopify help center'},
              {content: 'Community forums'},
            ],
          },
        ]}
        name="Dharma"
        detail="Jaded Pixel"
        initials="D"
        open={userMenuOpen}
        onToggle={this.toggleState('userMenuOpen')}
      />
    );

    const topBar = (
      <TopBar
        showNavigationToggle
        searchField={<TopBar.SearchField placeholder="Search" value="" />}
        onNavigationToggle={() => this.toggleState('navigationVisible')}
        userMenu={userMenuMarkup}
      />
    );

    const navUserMenu = (
      <Navigation.UserMenu
        actions={[
          {
            items: [
              {content: 'Your profile', icon: 'profile'},
              {content: 'Log out', icon: 'logOut'},
            ],
          },
          {
            items: [
              {content: 'Shopify help center'},
              {content: 'Community forums'},
            ],
          },
        ]}
        name="Dharma"
        detail="Jaded Pixel"
        initials="D"
        open={false}
      />
    );

    const navigationMarkup = (
      <Navigation
        location={activeLocation}
        onDismiss={() => this.handleDismiss()}
        userMenu={navUserMenu}
      >
        <Navigation.Section
          items={[
            {
              label: 'Home',
              icon: 'home',
              onClick: this.toggleState('isLoading'),
            },
            {
              label: 'Orders',
              icon: 'orders',
              onClick: this.toggleState('isLoading'),
            },
            {
              label: 'Products',
              icon: 'products',
              onClick: this.toggleState('isLoading'),
            },
            {
              label: 'Customers',
              icon: 'customers',
              onClick: this.toggleState('isLoading'),
              url: activeLocation,
            },
            {
              label: 'Analytics',
              icon: 'analytics',
              onClick: this.toggleState('isLoading'),
            },
            {
              label: 'Marketing',
              icon: 'marketing',
              onClick: this.toggleState('isLoading'),
            },
            {
              label: 'Apps',
              icon: 'apps',
              onClick: this.toggleState('isLoading'),
            },
          ]}
        />
        <Navigation.Section
          fill
          title="Sales channels"
          items={[
            {
              label: 'Online Store',
              icon: 'onlineStore',
              onClick: this.toggleState('isLoading'),
            },
          ]}
          separator
          action={{
            icon: 'circlePlusOutline',
            accessibilityLabel: 'Add a sales channel',
            onClick: this.toggleState('isLoading'),
          }}
        />
        <Navigation.Section
          items={[
            {
              label: 'Settings',
              icon: 'settings',
              onClick: this.toggleState('isLoading'),
            },
          ]}
        />
      </Navigation>
    );

    const loadingPageMarkup = (
      <SkeletonPage>
        <Layout>
          <Layout.Section>
            <Card sectioned>
              <TextContainer>
                <SkeletonDisplayText size="small" />
                <SkeletonBodyText lines={9} />
              </TextContainer>
            </Card>
          </Layout.Section>
        </Layout>
      </SkeletonPage>
    );

    const actualPageMarkup = (
      <Page title="Customers">
        <Layout>
          <Layout.Section>
            <Card>
              <ResourceList
                resourceName={resourceName}
                items={items}
                renderItem={this.renderItem}
                selectedItems={this.state.selectedItems}
                onSelectionChange={this.handleSelectionChange}
                promotedBulkActions={promotedBulkActions}
                bulkActions={bulkActions}
              />
            </Card>
          </Layout.Section>
        </Layout>
      </Page>
    );

    const loadingMarkup = isLoading ? <Loading /> : null;

    const pageMarkup = isLoading ? loadingPageMarkup : actualPageMarkup;

    return (
      <AppProvider
        theme={{
          logo: {
            width: 104,
            topBarSource:
              'https://cdn.shopify.com/shopify-marketing_assets/static/shopify-full-color-white.svg',
            contextualSaveBarSource:
              'https://cdn.shopify.com/shopify-marketing_assets/static/shopify-full-color-black.svg',
          },
          colors: {
            topBar: {
              color: '#f9fafb',
              background: 'rgb(28, 34, 96)',
              backgroundDarker: 'rgb(0, 8, 75)',
              backgroundLighter: 'rgb(67, 70, 127)',
            },
          },
        }}
      >
        <Frame
          showMobileNavigation={navigationVisible}
          navigation={navigationMarkup}
          topBar={topBar}
          onNavigationDismiss={() => this.handleDismiss()}
        >
          {loadingMarkup}
          {pageMarkup}
        </Frame>
      </AppProvider>
    );
  }

  toggleState = (key) => {
    return () => {
      this.setState((prevState) => ({[key]: !prevState[key]}));
    };
  };

  handleDismiss = () => {
    this.setState({navigationVisible: false});
  };

  renderItem = (item) => {
    const {id, url, name, location} = item;
    const media = <Avatar customer size="medium" name={name} />;

    return (
      <ResourceList.Item
        id={id}
        url={url}
        media={media}
        accessibilityLabel={`View details for ${name}`}
      >
        <h3>
          <TextStyle variation="strong">{name}</TextStyle>
        </h3>
        <div>{location}</div>
      </ResourceList.Item>
    );
  };

  handleSelectionChange = (selectedItems) => {
    this.setState({selectedItems});
  };
}
```

---

## Related components

- To display the navigation component on small screens, to provide search and a user menu, or to style the [frame](/components/structure/frame) component to reflect an application’s brand, use the [top bar](/components/structure/top-bar) component.
- To display the primary navigation within the frame of a non-embedded application, use the [navigation](/components/structure/navigation) component.
- To tell merchants their options once they have made changes to a form on the page use the [contextual save bar](/components/forms/contextual-save-bar) component.
- To provide quick, at-a-glance feedback on the outcome of an action, use the [toast](/components/feedback-indicators/toast) component.
- To indicate to merchants that a page is loading or an upload is processing use the [loading](/components/feedback-indicators/loading) component.
