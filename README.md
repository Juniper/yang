# Yang
Yang models for the Junos platform

#Usage
The Yang files are tested for compilation with pyang v1.5. Before consumption by an NMS, this requires changes to handle non-YANG Junos data models.

##### For use with NCS:
1. Import [Junos tailf extension](https://github.com/tail-f-systems/JNC/blob/master/examples/2-junos/tailf-common.yang) and include it in the main module
```
 module junos {
  namespace "http://xml.juniper.net/xnm/1.1/xnm";
  prefix junos;

  import tailf-common {
    prefix tailf;
  }
```

For example, to handle key enumeration leafs:

```
list community {
   key "community-name";
   ordered-by user;
   description "BGP community properties associated with a route";
   choice community-action {
      case case_1 {
        leaf equal-literal {
           description "Set the BGP communities in the route";
           type empty;
        }
      }
      case case_2 {
        leaf set {
           description "Set the BGP communities in the route";
           type empty;
        }
      }
      case case_3 {
        leaf plus-literal {
           description "Add BGP communities to the route";
           type empty;
        }
      }
      case case_4 {
        leaf add {
           description "Add BGP communities to the route";
           type empty;
        }
      }
      case case_5 {
        leaf minus-literal {
           description "Remove BGP communities from the route";
           type empty;
        }
      }
      case case_6 {
        leaf delete {
           description "Remove BGP communities from the route";
           type empty;
        }
      }
   }
   leaf community-name {
      description "Name to identify a BGP community";
      type string;
   }
}
```

changes to

```
list community {
    key "key1 community-name";
    ordered-by user;
    description "BGP community properties associated with a route";
    leaf key1 {
       tailf:junos-val-as-xml-tag;
       type enumeration {
          enum equal-literal {
              description "Set the BGP communities in the route";
          }
          enum set {
              description "Set the BGP communities in the route";
          }
          enum plus-literal {
              description "Add BGP communities to the route";
          }
          enum add {
              description "Add BGP communities to the route";
          }
          enum minus-literal {
              description "Remove BGP communities from the route";
          }
          enum delete {
              description "Remove BGP communities from the route";
          }
       }
    }
	  leaf community-name {
       description "Name to identify a BGP community";
       type string;
    }
}
```


# Contact
yang-support@juniper.net
