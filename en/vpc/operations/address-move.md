# Moving a public IP address to a different folder

You can move [public IP addresses](../concepts/address.md) between folders within a single [cloud](../../resource-manager/concepts/resources-hierarchy.md).

{% list tabs %}

- Management console

   1. In the [management console]({{ link-console-main }}), go to the folder where the address is located.
   1. In the list of services, select **{{ ui-key.yacloud.iam.folder.dashboard.label_vpc }}**.
   1. In the left-hand panel, select ![image](../../_assets/vpc/ip-addresses.svg) **{{ ui-key.yacloud.vpc.switch_addresses }}**.
   1. Click ![image](../../_assets/options.svg) in the row of the address to be moved and select **{{ ui-key.yacloud.vpc.button_move-vpc-object }}**.
   1. In the window that opens, select the destination folder.
   1. Click **{{ ui-key.yacloud.vpc.button_move-vpc-object }}**.

- CLI

   {% include [include](../../_includes/cli-install.md) %}

   {% include [default-catalogue](../../_includes/default-catalogue.md) %}

   1. View a description of the CLI move address command:

      ```bash
      yc vpc address move --help
      ```

   1. Get the name or ID of the address to move:

      ```bash
      yc vpc address list
      ```
      Result:
      ```text
      +----------------------+------+---------------+----------+-------+
      |          ID          | NAME |    ADDRESS    | RESERVED | USED  |
      +----------------------+------+---------------+----------+-------+
      | e2l50m7qo8gp******** |      | 84.252.137.20 | true     | false |
      | e9b0qnmuh2cb******** |      | 51.250.65.244 | true     | false |
      | e9br252il3ce******** |      | 51.250.68.195 | false    | true  |
      +----------------------+------+---------------+----------+-------+
      ```

   1. Get a list of available folders:

      ```bash
      yc resource-manager folder list
      ```

      Result:
      ```text
      +----------------------+------------------------+--------+--------+
      |          ID          |          NAME          | LABELS | STATUS |
      +----------------------+------------------------+--------+--------+
      | b1cs8ie21pk1******** | default                |        | ACTIVE |
      | b1chgf288nvg******** | my-folder-1            |        | ACTIVE |
      | b1cu6g9ielh6******** | my-folder-2            |        | ACTIVE |
      +----------------------+------------------------+--------+--------+
      ```

   1. Move the address by specifying the name or ID of the address and destination folder:

      ```bash
      yc vpc address move <address_name_or_ID> \
        --destination-folder-name <name_of_destination_folder> \
        --destination-folder-id <ID_of_destination_folder>
      ```
      Use either the `--destination-folder-name` parameter or the `--destination-folder-id` parameter.

      If the address is not in the current folder (default folder), specify the source folder using the `--folder-name` or `--folder-id` option.

      Result:

      ```text
       id: e9br252il3ce********
       folder_id: b1chgf288nvg********
       created_at: "2022-10-10T05:38:43Z"
       external_ipv4_address:
         address: 51.250.68.195
         zone_id: {{ region-id }}-a
         requirements: {}
       used: true
       type: EXTERNAL
       ip_version: IPV4
      ```

      For more information about the `yc vpc address move` command, see the [CLI reference](../../cli/cli-ref/managed-services/vpc/address/move.md).

- API

   To move a [public IP address](../concepts/address.md#public-addresses) to a different folder, use the [move](../api-ref/Address/move.md) REST API method for the [Address](../api-ref/Address/index.md) resource or the [AddressService/Move](../api-ref/grpc/address_service.md#Move) gRPC API call, and provide the following in the request:

    * ID of the address to move, in the `addressId` parameter.

      {% include [get-address-id](../../_includes/vpc/get-adress-id.md) %}

    * ID of the folder the gateway will be moved to, in the `destinationFolderId` parameter.

      {% include [get-catalog-id](../../_includes/get-catalog-id.md) %}

{% endlist %}

## Examples {#examples}

### Address in the current folder {#from-default-folder}

Move an address from the current folder by specifying the address name and destination folder name:

{% list tabs %}

- CLI

   ```bash
   yc vpc address move site-1 \
     --destination-folder-name my-folder-1
   ```

{% endlist %}

### Address in a different folder {#from-another-folder}

Move an address from a different folder. Specify the address ID and the source and destination folder IDs:

{% list tabs %}

- CLI

   ```bash
   yc vpc address move e9br252il3ce******** \
     --folder-id b1chgf288nvg******** \
     --destination-folder-id b1cs8ie21pk1********
   ```

{% endlist %}

