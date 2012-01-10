--- 
layout: post
title: Setting up Magento Community Shipping Module
tags: 
- Magento
- Module
- PHP
- Programming
- Shipping
status: publish
type: post
published: true
author: hleinone
---

I've been trying to create a shipping module of my own to [Magento](http://www.magentocommerce.com/) webstore. I was familiar with PHP but the Zend framework was (and still is) new to me. Being too lazy to read any documentation (in case of Magento it rarely exists or is outdated) or search the forums, I took the trial and error perspective and some example modules.

The forthcoming examples are heavily based on [CollinsHarper](http://www.collinsharper.com/)'s [Store Pickup Shipping Module](http://www.magentocommerce.com/extension/606/store-pickup-shipping-module) and verified to work with Magento 1.1.8. Practically it's just the same module as  a community module. In case of anyone from CollinsHarper reading, as your module is licenced as [beerware](http://en.wikipedia.org/wiki/Beerware), I hereby state that if you ever visit Helsinki I'll definitely buy you a beer.

The Store Pickup Shipping Module consists of four files.

*Use app/etc/modules/module name.xml*

`app/etc/modules/StorePickup_Shipping.xml`:

{% highlight xml %}
<?xml version="1.0"?>
<config>
    <modules>
        <!-- Use module name_Shipping -->
        <StorePickup_Shipping>
            <active>true</active>
            <codePool>community</codePool>
        <!-- Use module name_Shipping -->
        </StorePickup_Shipping>
    </modules>
</config>
{% endhighlight %}

*Use app/code/community/module name/Shipping/etc/config.xml*

`app/code/community/StorePickup/Shipping/etc/config.xml`:

{% highlight xml %}
<?xml version="1.0"?>
<config>
    <default>
        <carriers>
            <!-- Use group alias -->
            <storepickupmodule>
                <active>1</active>
                <!-- Use method name -->
                <allowed_methods>pickup</allowed_methods>
                <!-- Use method name -->
                <methods>pickup</methods>
                <sallowspecific>0</sallowspecific>
                <!-- Use module name_Shipping_Model_Carrier_class name -->
                <model>StorePickup_Shipping_Model_Carrier_StorePickup</model>
                <name>Store Pickup</name>
                <title>Store Pickup</title>
                <specificerrmsg>
                    This shipping method is currently unavailable.
                    If you would like to ship using this shipping
                    method, please contact us.
                </specificerrmsg>
                <handling_type>F</handling_type>
            <!-- Use group alias -->
            </storepickupmodule>
        </carriers>
    </default>
    <modules>
        <!-- declare module's version information -->
        <!-- Use module name_Shipping -->
        <StorePickup_Shipping>
            <!-- this version number will be used for database upgrades -->
            <version>0.1.0</version>
        <!-- Use module name_Shipping -->
        </StorePickup_Shipping>
    </modules>

    <global>
        <!-- declare model group for new module -->
        <models>
            <!-- model group alias to be used in Mage::getModel() -->
            <!-- Use group alias -->
            <storepickupmodule>
                <!-- base class name for the model group -->
                <!-- Use module name_Shipping_Model -->
                <class>StorePickup_Shipping_Model</class>
            <!-- Use group alias -->
            </storepickupmodule>
        </models>
        <!-- declare resource setup for new module -->
        <resources>
            <!-- resource identifier -->
            <!-- Use group alias_setup -->
            <storepickupmodule_setup>
                <!-- specify that this resource is a setup resource and used for upgrades -->
                <setup>
                    <!-- which module to look for install/upgrade files in -->
                    <!-- Use module name_Shipping_Model -->
                    <module>StorePickup_Shipping</module>
                </setup>
                <!-- specify database connection for this resource -->
                <connection>
                    <!-- do not create new connection, use predefined core setup connection -->
                    <use>core_setup</use>
                </connection>
            <!-- Use group alias_setup -->
            </storepickupmodule_setup>
        </resources>
    </global>
</config>
{% endhighlight %}

*Use app/code/community/module name/Shipping/etc/system.xml*

`app/code/community/StorePickup/Shipping/etc/system.xml`:

{% highlight xml %}
<?xml version="1.0"?>
<config>
    <sections>
        <carriers>
            <groups>
                <!-- Use group alias -->
                <storepickupmodule translate="label" module="shipping">
                    <label>Store Pickup</label>
                    <frontend_type>text</frontend_type>
                    <sort_order>13</sort_order>
                    <show_in_default>1</show_in_default>
                    <show_in_website>1</show_in_website>
                    <show_in_store>1</show_in_store>
                    <fields>
                        <active translate="label">
                            <label>Enabled</label>
                            <frontend_type>select</frontend_type>
                            <source_model>adminhtml/system_config_source_yesno</source_model>
                            <sort_order>1</sort_order>
                            <show_in_default>1</show_in_default>
                            <show_in_website>1</show_in_website>
                            <show_in_store>1</show_in_store>
                        </active>
                        <sort_order translate="label">
                            <label>Sort order</label>
                            <frontend_type>text</frontend_type>
                            <sort_order>100</sort_order>
                            <show_in_default>1</show_in_default>
                            <show_in_website>1</show_in_website>
                            <show_in_store>1</show_in_store>
                        </sort_order>
                        <title translate="label">
                            <label>Title</label>
                            <frontend_type>text</frontend_type>
                            <sort_order>2</sort_order>
                            <show_in_default>1</show_in_default>
                            <show_in_website>1</show_in_website>
                            <show_in_store>1</show_in_store>
                        </title>
                        <methodtitle translate="label">
                            <label>Method Title</label>
                            <frontend_type>text</frontend_type>
                            <sort_order>2</sort_order>
                            <show_in_default>1</show_in_default>
                            <show_in_website>1</show_in_website>
                            <show_in_store>1</show_in_store>
                        </methodtitle>
                        <handling translate="label">
                            <label>Handling fee</label>
                            <frontend_type>text</frontend_type>
                            <sort_order>12</sort_order>
                            <show_in_default>1</show_in_default>
                            <show_in_website>1</show_in_website>
                            <show_in_store>1</show_in_store>
                        </handling>
                        <handling_type translate="label">
                            <label>Calculate Handling Fee</label>
                            <frontend_type>select</frontend_type>
                            <source_model>shipping/source_handlingType</source_model>
                            <sort_order>10</sort_order>
                            <show_in_default>1</show_in_default>
                            <show_in_website>1</show_in_website>
                            <show_in_store>0</show_in_store>
                        </handling_type>

                        <sallowspecific translate="label">
                            <label>Ship to applicable countries</label>
                            <frontend_type>select</frontend_type>
                            <sort_order>90</sort_order>
                            <frontend_class>shipping-applicable-country</frontend_class>
                            <source_model>adminhtml/system_config_source_shipping_allspecificcountries</source_model>
                            <show_in_default>1</show_in_default>
                            <show_in_website>1</show_in_website>
                            <show_in_store>1</show_in_store>
                        </sallowspecific>
                        <specificcountry translate="label">
                            <label>Ship to Specific countries</label>
                            <frontend_type>multiselect</frontend_type>
                            <sort_order>91</sort_order>
                            <source_model>adminhtml/system_config_source_country</source_model>
                            <show_in_default>1</show_in_default>
                            <show_in_website>1</show_in_website>
                            <show_in_store>1</show_in_store>
                        </specificcountry>
                        <specificerrmsg translate="label">
                            <label>Displayed Error Message</label>
                            <frontend_type>textarea</frontend_type>
                            <sort_order>80</sort_order>
                            <show_in_default>1</show_in_default>
                            <show_in_website>1</show_in_website>
                            <show_in_store>1</show_in_store>
                        </specificerrmsg>
                    </fields>
                <!-- Use group alias -->
                </storepickupmodule>
            </groups>
        </carriers>
    </sections>
</config>
{% endhighlight %}

*Use app/code/community/module name/Shipping/Model/Carrier/class name.php*

`app/code/community/StorePickup/Shipping/Model/Carrier/StorePickup.php`:

{% highlight php %}
<?php

/*
* use at your own risk!
* This is totally beerware, they wouldn't let me select that on the Mag. Connect site. But I'm still standing by it!
*/
/* Use module name_Shipping_Model_Carrier_class name */
class StorePickup_Shipping_Model_Carrier_StorePickup extends Mage_Shipping_Model_Carrier_Abstract
{
    /* Use group alias */
    protected $_code = 'storepickupmodule';

    public function collectRates(Mage_Shipping_Model_Rate_Request $request)
    {
        // skip if not enabled
        if (!Mage::getStoreConfig('carriers/'.$this->_code.'/active'))
            return false;

        $result = Mage::getModel('shipping/rate_result');
        $handling = 0;
        if(Mage::getStoreConfig('carriers/'.$this->_code.'/handling') >0)
            $handling = Mage::getStoreConfig('carriers/'.$this->_code.'/handling');
        if(Mage::getStoreConfig('carriers/'.$this->_code.'/handling_type') == 'P' &amp;amp;&amp;amp; $request->getPackageValue() > 0)
            $handling = $request->getPackageValue()*$handling;

        $method = Mage::getModel('shipping/rate_result_method');
        $method->setCarrier($this->_code);
        $method->setCarrierTitle(Mage::getStoreConfig('carriers/'.$this->_code.'/title'));
        /* Use method name */
        $method->setMethod('pickup');
        $method->setMethodTitle(Mage::getStoreConfig('carriers/'.$this->_code.'/methodtitle'));
        $method->setCost($handling);
        $method->setPrice($handling);
        $result->append($method);
        return $result; // it doesnt do anything if there was an error just returns blank - maybe there should be a default shipping incase of problem? or email sysadmin?
    }
}
?>
{% endhighlight %}

The biggest problems I faced with the naming conventions, so incase you're interested in creating a shipping module of your own, note these.

* module name (StorePickup)
* class name (StorePickup)
* group alias (storepickupmodule)
* method name (pickup)

I have marked all occurences of these as comments on the files. These occur also in the file paths. Replace all these with the ones from your module if you wish.
