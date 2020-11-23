
## Integrate with AEM 6.5.x

This documentation is specific to `AEM on premise` version 6.5.x, 
to integrate with `AEM as cloud service`, please refer to our [`AEM as cloud service` documentation](aem_skyline_install.md).

To install and configure the AEM event proxy package on version 6.5.x:

1. Download the latest version of the package [version 6.5.6](https://github.com/AdobeDocs/adobeio-events/files/3729022/aem-event-proxy-6.5.6.zip) 
2. [Install this package](aem_skyline_package_install.md)
3. Configure the [`AEM Link Externalizer`](aem_on_premise_link_externalizer.md)
4. Configure Adobe I/O Credentials
   1. [set up your service user keystore in AEM](aem_keystore_setup.md) 
   2. [set up your workspace in Adobe I/O Developer console, and as OSGI configuration](aem_console_setup.md)
   3. [Finalize the Adobe IMS configuration in AEM](aem_ims_config.md)
5. Optionally you may
   1. perform a few [health checks](aem_on_premise_healthcheck.md)
   2. do some more [configuration fine tuning](aem_advanced_configurations.md)
   
