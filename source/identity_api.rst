


认证服务
====================

.. toctree::
   :maxdepth: 2
   :numbered:



概要
--------------------

认证服务提供认证、授权，基于OpenStack Keystone V2.0 API。



核心概念
------------------------



用户账号(User)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

用户账号有邮箱、用户名和密码，用于登陆 OpenStack 的 Web 管理面板。


API规范
--------------------------

认证服务API入口与API列表如下：


    * API入口: http://cdnapi.ztgame.com.cn
    * API列表:

+------------------+------------------------------+------------+--------------------------------+
|  资源            |  操作                        |  HTTP方法  |  URI路径                       |
+==================+==============================+============+================================+
|  Tokens (令牌)   |  :ref:`keystone-get-token`   |  POST      |  /cdn_api/tokens               |
+------------------+------------------------------+------------+--------------------------------+


令牌 (Tokens)
--------------------------------------

.. _keystone-get-token:

认证并生成新令牌(已可用)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

+------------+-------------------+---------------+
|  HTTP方法  |  URI路径          |  描述         |
+============+===================+===============+
|  POST      |  /cdn_api/tokens     |  获取令牌     |
+------------+-------------------+---------------+

请求参数

+-----------------------+----------+-----------------------------------------------------------------------------+
|  字段名               |  类型    |  描述                                                                       |
+=======================+==========+=============================================================================+
|  auth                 |  dict    |  认证信息(passwordCredentials)                                              |
+-----------------------+----------+-----------------------------------------------------------------------------+
|  passwordCredentials  |  dict    |  认证信息，包含username和password                                           |
+-----------------------+----------+-----------------------------------------------------------------------------+
|  username             |  string  |  用户名                                                                     |
+-----------------------+----------+-----------------------------------------------------------------------------+
|  password             |  string  |  密码                                                                       |
+-----------------------+----------+-----------------------------------------------------------------------------+

请求样例

::

    
    curl -i -s \
         -X POST https://222.73.155.185:5001/v3/auth/tokens \
         -H "Content-Type:application/json" \
         -d '{
                "auth": {
                    "passwordCredentials": {
                        "username": "test",
                        "password": "123456"
                    }
                }
            }'


返回参数

+-------------------+-------------+-------------------------------------------------------------------------+
|  字段名           |  类型       |  描述                                                                   |
+===================+=============+=========================================================================+
|  access           |  dict       |  包含了token, serviceCatalog, user, metadata等信息                      |
+-------------------+-------------+-------------------------------------------------------------------------+
|  token            |  dict       |  返回token信息，包含了token, tenant等信息                               |
+-------------------+-------------+-------------------------------------------------------------------------+
|  serviceCatalog   |  array      |  服务目录，包含了用户有权限的所有星云云服务的endpoints信息              |
+-------------------+-------------+-------------------------------------------------------------------------+
|  tenant           |  dict       |  租户信息，包含了租户名，租户ID信息                                     |
+-------------------+-------------+-------------------------------------------------------------------------+
|  user             |  dict       |  用户信息，包含了用户名，用户ID信息                                     |
+-------------------+-------------+-------------------------------------------------------------------------+
|  roles            |  array      |  该用户在该项目上的角色信息                                             |
+-------------------+-------------+-------------------------------------------------------------------------+
|  expires          |  timestamp  |  Token失效日期                                                          |
+-------------------+-------------+-------------------------------------------------------------------------+
|  issued_at        |  timestamp  |  Token创建日期                                                          |
+-------------------+-------------+-------------------------------------------------------------------------+
|  metadata         |  dict       |  包含roles和是否是管理员信息                                            |
+-------------------+-------------+-------------------------------------------------------------------------+
|  endpoints        |  array      |  星云云服务id, 类型 (type) 和endpoint信息(包含url, region)              |
+-------------------+-------------+-------------------------------------------------------------------------+

返回样例

::

    HTTP/1.0 200 OK
    
    {
        "access": {
            "token": {
                "issued_at": "2015-11-05T04:01:01.560490",
                "expires": "2015-11-05T05:01:01Z",
                "id": "cee261a1e2fa477d966d512b65f59010"
            },
            "serviceCatalog": [],
            "user": {
                "username": "test",
                "roles_links": [],
                "id": "24a254d2101f4329955da76f337e4653",
                "roles": [],
                "name": "test"
            },
            "metadata": {
                "is_admin": 0,
                "roles": []
            }
        }
    }
