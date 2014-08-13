---
layout: page
title: "Adding a new register to the app"
---

# How to add a new register to the app

The following steps are involved in adding a new register to the app

(Please note: This documentation is written for the Android Native Home screen and adding a new native register)

* The new native has to implemented as per the architecture defined in the [Native Register Architecture]({{root_url}}/dristhi_app/smart_registers/architecture_native_registers)
* The UI for the new register shall be defined in the corresponding layout xml file(s)
* The Register Activity shall instantiate the necessary controllers and the ClientProviders as required
* Once the register has been defined, it needs to be added to the home register for it to be launched
* To add a new button to the Home Screen, the description of then new button needs to be added to the layout file for the home screen 
* For example, the EC Register button is defined in the [smart_registers_home.xml](https://github.com/SEL-Columbia/dristhi-app/blob/master/dristhi-app/res/layout/smart_registers_home.xml#L26-56) as below:
```xml
<FrameLayout
        android:layout_width="0dp"
        android:layout_height="fill_parent"
        android:layout_weight="1">

    <ImageButton
            android:id="@+id/btn_ec_register"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:src="@drawable/register_ec"
            style="@style/ImageButton.Home.Register"/>

    <org.ei.drishti.view.customControls.CustomFontTextView
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="EC"
            android:textStyle="bold"
            style="@style/CustomFontTextViewStyle.Home.RegisterName"/>

    <org.ei.drishti.view.customControls.CustomFontTextView
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="REGISTER"
            style="@style/CustomFontTextViewStyle.Home.RegisterLabel"/>

    <org.ei.drishti.view.customControls.CustomFontTextView
            android:id="@+id/txt_ec_register_client_count"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            style="@style/CustomFontTextViewStyle.Badge"/>
</FrameLayout>
```
* Once the new button has been added to the home register, then the corresponding register activity has to be opened when the button is tapped
* This can be achived by creating a method in navigation controller which launches the new register activity
* Then in the [NativeHomeActivity](https://github.com/SEL-Columbia/dristhi-app/blob/master/dristhi-app/src/main/java/org/ei/drishti/view/activity/NativeHomeActivity.java#L185-211) the corresponding method is called based on the id of which button is tapped
```java
private View.OnClickListener onRegisterStartListener = new View.OnClickListener() {

    @Override
    public void onClick(View view) {
        switch (view.getId()) {
            case R.id.btn_ec_register:
                navigationController.startECSmartRegistry();
                break;

            case R.id.btn_anc_register:
                navigationController.startANCSmartRegistry();
                break;

            case R.id.btn_pnc_register:
                navigationController.startPNCSmartRegistry();
                break;

            case R.id.btn_child_register:
                navigationController.startChildSmartRegistry();
                break;

            case R.id.btn_fp_register:
                navigationController.startFPSmartRegistry();
                break;
        }
    }
}
```
* This would launch the new register
