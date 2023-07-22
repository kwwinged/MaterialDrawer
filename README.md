#MaterialDrawer  [![Maven Central](https://maven-badges.herokuapp.com/maven-central/com.mikepenz/materialdrawer/badge.svg?style=flat)](https://maven-badges.herokuapp.com/maven-central/com.mikepenz/materialdrawer) [![Android Arsenal](https://img.shields.io/badge/Android%20Arsenal-MaterialDrawer-brightgreen.svg?style=flat)](https://android-arsenal.com/details/1/1526) [![Join the chat at https://gitter.im/mikepenz/MaterialDrawer](https://badges.gitter.im/Join%20Chat.svg)]

![MaterialDrawer the flexible, easy to use, all in one drawer library for your android project.]

> Does your application **contain a Drawer**? Do you want to have it **up and running in less than 5 minutes**? Do you want your drawer to follow the **Android Design Guidelines**?
Do you have **profiles**? Do you need **flexibility**? Is Google's navigation Drawer of the **design support** not enough for you? Do you want a **simple and easy** to understand api?

If any (or all) of these questions seem familiar, the **MaterialDrawer** is the perfect library for you.

**Never** waste your time again.
It provides you with the easiest possible implementation of a navigation drawer for your application.
There  is a Header with profiles (**AccountHeader**), a **MiniDrawer** for Tablets (like Gmail), provide
**custom DrawerItems**, **custom colors**, **custom themes**, ... **No limits** for customizations.

###A quick overview what's in
- **the easiest possible integration**
- integrate in less than **5 minutes**
- compatible down to **API Level 14**
- includes an **AccountSwitcher**
- quick and simple api
- follows the **Google Material Design Guidelines**
- use **vector** (.svg) icons and **icon fonts** via the Android-Iconics integration
 - **Google Material Design Icons**, Google **Material Community** Design Icons, FontAwesome and more
- comes with various **themes** which help to get your own themes clean
- modify the colors on the go
- **uses the AppCompat support library**
- comes with multiple default drawer items
- based on a **RecyclerView**
- **RTL** support
- Gmail like **MiniDrawer**
- expandable items
- **badge** support
- define custom drawer items
- tested and **stable**
- sticky footer or headers
- **absolutely NO limits**

#Setup
##1. Provide the gradle dependency

```gradle
compile('com.mikepenz:materialdrawer:5.8.1@aar') {
	transitive = true
}
```

##2. Add your drawer
```java
new DrawerBuilder().withActivity(this).build();
```

Great. Your drawer is now ready to use.


#Additional Setup
##Add items and adding some functionality

```java
//if you want to update the items at a later time it is recommended to keep it in a variable
PrimaryDrawerItem item1 = new PrimaryDrawerItem().withIdentifier(1).withName(R.string.drawer_item_home);
SecondaryDrawerItem item2 = new SecondaryDrawerItem().withIdentifier(2).withName(R.string.drawer_item_settings);

//create the drawer and remember the `Drawer` result object
Drawer result = new DrawerBuilder()
    .withActivity(this)
    .withToolbar(toolbar)
    .addDrawerItems(
	    item1,
	    new DividerDrawerItem(),
	    item2,
	    new SecondaryDrawerItem().withName(R.string.drawer_item_settings)
    )
    .withOnDrawerItemClickListener(new Drawer.OnDrawerItemClickListener() {
        @Override
        public boolean onItemClick(View view, int position, IDrawerItem drawerItem) {
    	    // do something with the clicked item :D
        }
    })
    .build();
```

##Selecting an item
```java
//set the selection to the item with the identifier 1
result.setSelection(1);
//set the selection to the item with the identifier 2
result.setSelection(item2);
//set the selection and also fire the `onItemClick`-listener
result.setSelection(1, true);
```

By default, when a drawer item is clicked, it becomes the new selected item. If this isn't the expected behavior,
you can disable it for this item using `withSelectable(false)`:
```java
new SecondaryDrawerItem().withName(R.string.drawer_item_dialog).withSelectable(false)
```

##Modify items or the drawer

```java
//modify an item of the drawer
item1.withName("A new name for this drawerItem").withBadge("19").withBadgeStyle(new BadgeStyle().withTextColor(Color.WHITE).withColorRes(R.color.md_red_700));
//notify the drawer about the updated element. it will take care about everything else
result.updateItem(item1);

//to update only the name, badge, icon you can also use one of the quick methods
result.updateName(1, "A new name");

//the result object also allows you to add new items, remove items, add footer, sticky footer, ..
result.addItem(new DividerDrawerItem());
result.addStickyFooterItem(new PrimaryDrawerItem().withName("StickyFooterItem"));

//remove items with an identifier
result.removeItem(2);

//open / close the drawer
result.openDrawer();
result.closeDrawer();

//get the reference to the `DrawerLayout` itself
result.getDrawerLayout();
```

##Add profiles and an AccountHeader
```java
// Create the AccountHeader
AccountHeader headerResult = new AccountHeaderBuilder()
	.withActivity(this)
    .withHeaderBackground(R.drawable.header)
	.addProfiles(
		new ProfileDrawerItem().withName("Mike Penz").withEmail("mikepenz@gmail.com").withIcon(getResources().getDrawable(R.drawable.profile))
	)
    .withOnAccountHeaderListener(new AccountHeader.OnAccountHeaderListener() {
		@Override
		public boolean onProfileChanged(View view, IProfile profile, boolean currentProfile) {
		    return false;
		}
	})
	.build();

//Now create your drawer and pass the AccountHeader.Result
new DrawerBuilder()
    .withAccountHeader(headerResult)
    //additional Drawer setup as shown above
    ...
    .build();

```

##Use the included icon font
The MaterialDrawer comes with the `core` of the [Android-Iconics](https://github.com/mikepenz/Android-Iconics) library. This allows you to create your `DrawerItems` with an icon from any font.

Choose the fonts you need. [Available Fonts](https://github.com/mikepenz/Android-Iconics#2-choose-your-desired-fonts)
**build.gradle**
```gradle
compile 'com.mikepenz:google-material-typeface:x.y.z@aar' //Google Material Icons
compile 'com.mikepenz:fontawesome-typeface:x.y.z@aar'     //FontAwesome
```

**java**
```java
//now you can simply use any icon of the Google Material Icons font
new PrimaryDrawerItem().withIcon(GoogleMaterial.Icon.gmd_wb_sunny)
//Or an icon from FontAwesome
new SecondaryDrawerItem().withIcon(FontAwesome.Icon.faw_github)
```

#Advanced Setup
##Activity with ActionBar
###Code:
```java
new DrawerBuilder()
	.withActivity(this)
	.withTranslucentStatusBar(false)
    .withActionBarDrawerToggle(false)
	.addDrawerItems(
		//pass your items here
	)
	.build();
```

##Activity with Multiple Drawers
###Code:
```java
Drawer result = new DrawerBuilder()
	.withActivity(this)
	.withToolbar(toolbar)
	.addDrawerItems(
		//pass your items here
	)
	.build();

new DrawerBuilder()
	.withActivity(this)
    .addDrawerItems(
    	//pass your items here
    )
    .withDrawerGravity(Gravity.END)
    .append(result);
```

##Load images via url
The MaterialDrawer supports fetching images from URLs and setting them for the Profile icons. As the MaterialDrawer does not contain an ImageLoading library
the dev can choose his own implementation (Picasso, Glide, ...). This has to be done, before the first image should be loaded via URL. (Should be done in the Application, but any other spot before loading the first image is working too)
* SAMPLE using [PICASSO](https://github.com/square/picasso)
* [SAMPLE](https://github.com/mikepenz/MaterialDrawer/blob/develop/app/src/main/java/com/mikepenz/materialdrawer/app/CustomApplication.java) using [GLIDE](https://github.com/bumptech/glide)

###Code:
```java
//initialize and create the image loader logic
DrawerImageLoader.init(new AbstractDrawerImageLoader() {
    @Override
    public void set(ImageView imageView, Uri uri, Drawable placeholder) {
        Picasso.with(imageView.getContext()).load(uri).placeholder(placeholder).into(imageView);
    }

    @Override
    public void cancel(ImageView imageView) {
        Picasso.with(imageView.getContext()).cancelRequest(imageView);
    }

    /*
    @Override
    public Drawable placeholder(Context ctx) {
        return super.placeholder(ctx);
    }

    @Override
    public Drawable placeholder(Context ctx, String tag) {
        return super.placeholder(ctx, tag);
    }
    */
});
```
