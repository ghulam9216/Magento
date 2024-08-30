# Problem
In Magento 2.4.7, when working with nested WYSIWYG editors, any content containing double quotes (") may lead to unexpected behavior. This is especially problematic when toggling between "Show" and "Hide" modes within the editor. The issue is traced back to the tinymce5Adapter.js file, which is responsible for handling the TinyMCE editor in Magento's admin panel.

# Solution
To resolve this issue, replace the existing **tinymce5Adapter.js** file located at:


# Which Changes need in code

**orignal function**
        toggle: function () {
            var content;

            if (!tinyMCE.get(this.getId())) {
                this.turnOn();

                return true;
            }

            content = this.get(this.getId()) ? this.get(this.getId()).getContent() : this.getTextArea().val();

            this.turnOff();

            if (content.match(/{{.+?}}/g)) {
                this.getTextArea().val(content.replace(/&quot;/g, '"'));
            }

            return false;
        }

  **Replace by Below Function**

          toggle: function () {
            var content;

            if (!tinyMCE.get(this.getId())) {
                this.turnOn();

                return true;
            }

            content = this.get(this.getId()) ? this.get(this.getId()).getContent() : this.getTextArea().val();

            this.turnOff();

            if (content.match(/{{.+?}}/g)) {
                this.getTextArea().val(content.replace(/&quot;/g, 'quot;'));
            }

            return false;
        }


**Blow vendor file replace in magento lib**
vendor/magento/magento2-base/lib/web/mage/adminhtml/wysiwyg/tiny_mce/tinymce5Adapter.js
->
lib/web/mage/adminhtml/wysiwyg/tiny_mce/tinymce5Adapter.js


Deploy static content (if necessary):
php bin/magento setup:static-content:deploy -f
php bin/magento cache:clean
php bin/magento cache:flush

# Compatibility
This fix is specifically for Magento 2.4.7. It may not be applicable to other versions of Magento without further testing.

# Contributing
If you encounter any issues or have suggestions for improvements, feel free to open an issue or submit a pull request.

# License
This project is licensed under the MIT License - see the LICENSE file for details.

This README provides clear instructions and context for anyone looking to resolve the nested WYSIWYG editor issue in Magento 2.4.7.



