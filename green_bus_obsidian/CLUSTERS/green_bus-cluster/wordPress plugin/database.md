---
tags:
  - green_bus-cluster
parent: "[[wordPress plugin|wordPress plugin]]"
generation: 2
---
# Queries

- select all buses from `wp_posts`
```sql
SELECT wp_posts.*
FROM wp_posts
WHERE 1=1
AND wp_posts.post_type = 'wbtm_bus'
AND ((wp_posts.post_status = 'publish'))
ORDER BY wp_posts.post_date DESC
```

- select the bus  information with id 27 from `wp_postmeta`  table
```sql
SELECT post_id, meta_key, meta_value
FROM wp_postmeta
WHERE post_id  = 27
ORDER BY meta_id ASC
```

# Arrays

## Seat plan array

1. the array is serialized PHP array 
```
a:3:{i:0;a:4:{s:5:"seat1";s:2:"A1";s:5:"seat2";s:2:"A2";s:5:"seat3";s:0:"";s:5:"seat4";s:2:"A3";}i:1;a:4:{s:5:"seat1";s:2:"B1";s:5:"seat2";s:2:"B2";s:5:"seat3";s:0:"";s:5:"seat4";s:2:"B3";}i:2;a:4:{s:5:"seat1";s:2:"C1";s:5:"seat2";s:2:"C2";s:5:"seat3";s:0:"";s:5:"seat4";s:2:"C3";}}
```

2. after after Deserialize it
```php
Array
(
    [0] => Array
        (
            [seat1] => A1
            [seat2] => A2
            [seat3] => 
            [seat4] => A3
        )

    [1] => Array
        (
            [seat1] => B1
            [seat2] => B2
            [seat3] => 
            [seat4] => B3
        )

    [2] => Array
        (
            [seat1] => C1
            [seat2] => C2
            [seat3] => 
            [seat4] => C3
        )
)

```

3. converting the array to json

```json
[
    {
        "seat1": "A1",
        "seat2": "A2",
        "seat3": "",
        "seat4": "A3"
    },
    {
        "seat1": "B1",
        "seat2": "B2",
        "seat3": "",
        "seat4": "B3"
    },
    {
        "seat1": "C1",
        "seat2": "C2",
        "seat3": "",
        "seat4": "C3"
    }
]

```

4. converting the array to JavaScript array

```js
const seats = [
    { seat1: "A1", seat2: "A2", seat3: "", seat4: "A3" },
    { seat1: "B1", seat2: "B2", seat3: "", seat4: "B3" },
    { seat1: "C1", seat2: "C2", seat3: "", seat4: "C3" }
];
```

## Route array

1. the array is serialized PHP array 
```
a:4:{i:0;a:4:{s:5:"place";s:5:"Paris";s:4:"time";s:5:"09:00";s:4:"type";s:2:"bp";s:8:"next_day";b:0;}i:1;a:4:{s:5:"place";s:9:"Frankfurt";s:4:"time";s:5:"10:30";s:4:"type";s:4:"both";s:8:"next_day";b:0;}i:2;a:4:{s:5:"place";s:7:"Hamburg";s:4:"time";s:5:"12:00";s:4:"type";s:4:"both";s:8:"next_day";b:0;}i:3;a:4:{s:5:"place";s:6:"Berlin";s:4:"time";s:5:"01:30";s:4:"type";s:2:"dp";s:8:"next_day";b:0;}}
```

1. after after Deserialize it

> [!info] type
> bp = boarding point
dp = dropping point
both = boarding point+dropping point

```php
Array
(
    [0] => Array
        (
            [place] => Paris
            [time] => 09:00
            [type] => bp
            [next_day] => 
        )

    [1] => Array
        (
            [place] => Frankfurt
            [time] => 10:30
            [type] => both
            [next_day] => 
        )

    [2] => Array
        (
            [place] => Hamburg
            [time] => 12:00
            [type] => both
            [next_day] => 
        )

    [3] => Array
        (
            [place] => Berlin
            [time] => 01:30
            [type] => dp
            [next_day] => 
        )

)

```

## Price array

1. the array is serialized PHP array 
```
a:6:{i:0;a:5:{s:22:"wbtm_bus_bp_price_stop";s:5:"Paris";s:22:"wbtm_bus_dp_price_stop";s:9:"Frankfurt";s:14:"wbtm_bus_price";s:2:"10";s:20:"wbtm_bus_child_price";s:0:"";s:21:"wbtm_bus_infant_price";s:0:"";}i:1;a:5:{s:22:"wbtm_bus_bp_price_stop";s:5:"Paris";s:22:"wbtm_bus_dp_price_stop";s:7:"Hamburg";s:14:"wbtm_bus_price";s:2:"15";s:20:"wbtm_bus_child_price";s:0:"";s:21:"wbtm_bus_infant_price";s:0:"";}i:2;a:5:{s:22:"wbtm_bus_bp_price_stop";s:5:"Paris";s:22:"wbtm_bus_dp_price_stop";s:6:"Berlin";s:14:"wbtm_bus_price";s:2:"20";s:20:"wbtm_bus_child_price";s:0:"";s:21:"wbtm_bus_infant_price";s:0:"";}i:3;a:5:{s:22:"wbtm_bus_bp_price_stop";s:9:"Frankfurt";s:22:"wbtm_bus_dp_price_stop";s:7:"Hamburg";s:14:"wbtm_bus_price";s:1:"7";s:20:"wbtm_bus_child_price";s:0:"";s:21:"wbtm_bus_infant_price";s:0:"";}i:4;a:5:{s:22:"wbtm_bus_bp_price_stop";s:9:"Frankfurt";s:22:"wbtm_bus_dp_price_stop";s:6:"Berlin";s:14:"wbtm_bus_price";s:2:"12";s:20:"wbtm_bus_child_price";s:0:"";s:21:"wbtm_bus_infant_price";s:0:"";}i:5;a:5:{s:22:"wbtm_bus_bp_price_stop";s:7:"Hamburg";s:22:"wbtm_bus_dp_price_stop";s:6:"Berlin";s:14:"wbtm_bus_price";s:1:"8";s:20:"wbtm_bus_child_price";s:0:"";s:21:"wbtm_bus_infant_price";s:0:"";}}
```

1. after after Deserialize it

```php
Array
(
    [0] => Array
        (
            [wbtm_bus_bp_price_stop] => Paris
            [wbtm_bus_dp_price_stop] => Frankfurt
            [wbtm_bus_price] => 10
            [wbtm_bus_child_price] => 
            [wbtm_bus_infant_price] => 
        )

    [1] => Array
        (
            [wbtm_bus_bp_price_stop] => Paris
            [wbtm_bus_dp_price_stop] => Hamburg
            [wbtm_bus_price] => 15
            [wbtm_bus_child_price] => 
            [wbtm_bus_infant_price] => 
        )

    [2] => Array
        (
            [wbtm_bus_bp_price_stop] => Paris
            [wbtm_bus_dp_price_stop] => Berlin
            [wbtm_bus_price] => 20
            [wbtm_bus_child_price] => 
            [wbtm_bus_infant_price] => 
        )

    [3] => Array
        (
            [wbtm_bus_bp_price_stop] => Frankfurt
            [wbtm_bus_dp_price_stop] => Hamburg
            [wbtm_bus_price] => 7
            [wbtm_bus_child_price] => 
            [wbtm_bus_infant_price] => 
        )

    [4] => Array
        (
            [wbtm_bus_bp_price_stop] => Frankfurt
            [wbtm_bus_dp_price_stop] => Berlin
            [wbtm_bus_price] => 12
            [wbtm_bus_child_price] => 
            [wbtm_bus_infant_price] => 
        )

    [5] => Array
        (
            [wbtm_bus_bp_price_stop] => Hamburg
            [wbtm_bus_dp_price_stop] => Berlin
            [wbtm_bus_price] => 8
            [wbtm_bus_child_price] => 
            [wbtm_bus_infant_price] => 
        )

)
```

# Tables from database
## All buses table
|ID|post_author|post_date|post_date_gmt|post_content|post_title|post_excerpt|post_status|comment_status|ping_status|post_password|post_name|to_ping|pinged|post_modified|post_modified_gmt|post_content_filtered|post_parent|guid|menu_order|post_type|post_mime_type|comment_count|
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|[40](http://localhost:10005/?username=root&db=local&edit=wp_posts&where%5BID%5D=40)|1|2025-03-04 19:50:36|2025-03-04 19:50:36||asd||publish|closed|closed||asd|||2025-03-04 19:50:36|2025-03-04 19:50:36||0|[http://bus.local/?post_type=wbtm_bus&#038;p=40](http://bus.local/?post_type=wbtm_bus&#038;p=40)|0|wbtm_bus||0|
|[36](http://localhost:10005/?username=root&db=local&edit=wp_posts&where%5BID%5D=36)|1|2025-03-04 19:41:02|2025-03-04 19:41:02||ff||publish|closed|closed||ff|||2025-03-04 19:41:28|2025-03-04 19:41:28||0|[http://bus.local/?post_type=wbtm_bus&#038;p=36](http://bus.local/?post_type=wbtm_bus&#038;p=36)|0|wbtm_bus||0|
|[29](http://localhost:10005/?username=root&db=local&edit=wp_posts&where%5BID%5D=29)|1|2025-02-28 20:22:20|2025-02-28 20:22:20||Bold Bus||publish|closed|closed||bold-bus|||2025-02-28 20:22:20|2025-02-28 20:22:20||0|[http://bus.local/bus/bold-bus/](http://bus.local/bus/bold-bus/)|0|wbtm_bus||0|
|[27](http://localhost:10005/?username=root&db=local&edit=wp_posts&where%5BID%5D=27)|1|2025-02-28 20:22:20|2025-02-28 20:22:20||Royal Bus||publish|closed|closed||royal-bus|||2025-03-04 20:02:41|2025-03-04 20:02:41||0|[http://bus.local/bus/royal-bus/](http://bus.local/bus/royal-bus/)|0|wbtm_bus||0|
|[25](http://localhost:10005/?username=root&db=local&edit=wp_posts&where%5BID%5D=25)|1|2025-02-28 20:22:20|2025-02-28 20:22:20||Berlin Linien Bus||publish|closed|closed||berlin-linien-bus|||2025-02-28 20:22:20|2025-02-28 20:22:20||0|[http://bus.local/bus/berlin-linien-bus/](http://bus.local/bus/berlin-linien-bus/)|0|wbtm_bus||0|
|[23](http://localhost:10005/?username=root&db=local&edit=wp_posts&where%5BID%5D=23)|1|2025-02-28 20:22:20|2025-02-28 20:22:20||Bonanza Bus||publish|closed|closed||bonanza-bus|||2025-02-28 20:22:20|2025-02-28 20:22:20||0|[http://bus.local/bus/bonanza-bus/](http://bus.local/bus/bonanza-bus/)|0|wbtm_bus||0|
|[19](http://localhost:10005/?username=root&db=local&edit=wp_posts&where%5BID%5D=19)|1|2025-02-28 20:22:19|2025-02-28 20:22:19||BYD Express||publish|closed|closed||byd-express|||2025-02-28 20:22:19|2025-02-28 20:22:19||0|[http://bus.local/bus/byd-express/](http://bus.local/bus/byd-express/)|0|wbtm_bus||0|
|[17](http://localhost:10005/?username=root&db=local&edit=wp_posts&where%5BID%5D=17)|1|2025-02-28 20:22:19|2025-02-28 20:22:19||Mega Bus Express||publish|closed|closed||mega-bus-express|||2025-02-28 20:22:19|2025-02-28 20:22:19||0|[http://bus.local/bus/mega-bus-express/](http://bus.local/bus/mega-bus-express/)|0|wbtm_bus||0|
|[15](http://localhost:10005/?username=root&db=local&edit=wp_posts&where%5BID%5D=15)|1|2025-02-28 20:22:19|2025-02-28 20:22:19||Flix Bus Service||publish|closed|closed||flix-bus-service|||2025-02-28 20:22:19|2025-02-28 20:22:19||0|[http://bus.local/bus/flix-bus-service/](http://bus.local/bus/flix-bus-service/)|0|wbtm_bus||0|
## id = 27  Bus table

|post_id|meta_key|meta_value|
|---|---|---|
|27|link_wc_product|42|
|27|check_if_run_once|1|
|27|wbtm_bus_no|royal_706|
|27|wbtm_bus_category|AC|
|27|wbtm_seat_type_conf|wbtm_seat_plan|
|27|driver_seat_position|driver_left|
|27|wbtm_seat_rows|3|
|27|wbtm_seat_cols|4|
|27|wbtm_get_total_seat|9|
|27|wbtm_bus_seats_info|a:3:{i:0;a:4:{s:5:"seat1";s:2:"A1";s:5:"seat2";s:2:"A2";s:5:"seat3";s:0:"";s:5:"seat4";s:2:"A3";}i:1;a:4:{s:5:"seat1";s:2:"B1";s:5:"seat2";s:2:"B2";s:5:"seat3";s:0:"";s:5:"seat4";s:2:"B3";}i:2;a:4:{s:5:"seat1";s:2:"C1";s:5:"seat2";s:2:"C2";s:5:"seat3";s:0:"";s:5:"seat4";s:2:"C3";}}|
|27|show_upper_desk|no|
|27|wbtm_route_direction|a:4:{i:0;s:5:"Paris";i:1;s:9:"Frankfurt";i:2;s:7:"Hamburg";i:3;s:6:"Berlin";}|
|27|wbtm_bus_bp_stops|a:3:{i:0;s:5:"Paris";i:1;s:9:"Frankfurt";i:2;s:7:"Hamburg";}|
|27|wbtm_bus_next_stops|a:3:{i:0;s:9:"Frankfurt";i:1;s:7:"Hamburg";i:2;s:6:"Berlin";}|
|27|wbtm_route_info|a:4:{i:0;a:4:{s:5:"place";s:5:"Paris";s:4:"time";s:5:"09:00";s:4:"type";s:2:"bp";s:8:"next_day";b:0;}i:1;a:4:{s:5:"place";s:9:"Frankfurt";s:4:"time";s:5:"10:30";s:4:"type";s:4:"both";s:8:"next_day";b:0;}i:2;a:4:{s:5:"place";s:7:"Hamburg";s:4:"time";s:5:"12:00";s:4:"type";s:4:"both";s:8:"next_day";b:0;}i:3;a:4:{s:5:"place";s:6:"Berlin";s:4:"time";s:5:"01:30";s:4:"type";s:2:"dp";s:8:"next_day";b:0;}}|
|27|wbtm_bus_prices|a:6:{i:0;a:5:{s:22:"wbtm_bus_bp_price_stop";s:5:"Paris";s:22:"wbtm_bus_dp_price_stop";s:9:"Frankfurt";s:14:"wbtm_bus_price";s:2:"10";s:20:"wbtm_bus_child_price";s:0:"";s:21:"wbtm_bus_infant_price";s:0:"";}i:1;a:5:{s:22:"wbtm_bus_bp_price_stop";s:5:"Paris";s:22:"wbtm_bus_dp_price_stop";s:7:"Hamburg";s:14:"wbtm_bus_price";s:2:"15";s:20:"wbtm_bus_child_price";s:0:"";s:21:"wbtm_bus_infant_price";s:0:"";}i:2;a:5:{s:22:"wbtm_bus_bp_price_stop";s:5:"Paris";s:22:"wbtm_bus_dp_price_stop";s:6:"Berlin";s:14:"wbtm_bus_price";s:2:"20";s:20:"wbtm_bus_child_price";s:0:"";s:21:"wbtm_bus_infant_price";s:0:"";}i:3;a:5:{s:22:"wbtm_bus_bp_price_stop";s:9:"Frankfurt";s:22:"wbtm_bus_dp_price_stop";s:7:"Hamburg";s:14:"wbtm_bus_price";s:1:"7";s:20:"wbtm_bus_child_price";s:0:"";s:21:"wbtm_bus_infant_price";s:0:"";}i:4;a:5:{s:22:"wbtm_bus_bp_price_stop";s:9:"Frankfurt";s:22:"wbtm_bus_dp_price_stop";s:6:"Berlin";s:14:"wbtm_bus_price";s:2:"12";s:20:"wbtm_bus_child_price";s:0:"";s:21:"wbtm_bus_infant_price";s:0:"";}i:5;a:5:{s:22:"wbtm_bus_bp_price_stop";s:7:"Hamburg";s:22:"wbtm_bus_dp_price_stop";s:6:"Berlin";s:14:"wbtm_bus_price";s:1:"8";s:20:"wbtm_bus_child_price";s:0:"";s:21:"wbtm_bus_infant_price";s:0:"";}}|
|27|show_extra_service|yes|
|27|wbtm_extra_services|a:2:{i:0;a:4:{s:11:"option_name";s:13:"Welcome Drink";s:12:"option_price";s:2:"50";s:10:"option_qty";s:3:"500";s:15:"option_qty_type";s:8:"inputbox";}i:1;a:4:{s:11:"option_name";s:3:"Cap";s:12:"option_price";s:2:"70";s:10:"option_qty";s:3:"500";s:15:"option_qty_type";s:8:"inputbox";}}|
|27|show_pickup_point|no|
|27|wbtm_pickup_point|a:0:{}|
|27|show_operational_on_day|no|
|27|wbtm_particular_dates|a:1:{i:0;s:10:"1970-01-01";}|
|27|wbtm_repeated_start_date|2025-03-01|
|27|wbtm_repeated_after|1|
|27|wbtm_active_days|90|
|27|_edit_lock|1741119359:1|
|27|_edit_last|1|
|27|wbtm_registration|yes|
|27|wbtm_seat_rows_dd|0|
|27|wbtm_seat_cols_dd|0|
|27|wbtm_seat_dd_price_parcent||
|27|wbtm_bus_seats_info_dd|a:0:{}|
|27|wbtm_repeated_end_date||
|27|wbtm_off_days||
|27|wbtm_off_dates|a:0:{}|
|27|wbtm_offday_schedule|a:0:{}|
|27|wbtm_offday_range|a:0:{}|
|27|wbtm_pickup_point_required|no|
|27|wbtm_drop_off_point|a:0:{}|
|27|show_drop_off_point|no|
|27|wbtm_dropping_point_required|no|
|27|_tax_status|none|
|27|_tax_class|
