# Postman Configuration Guide

## Requirements

To explore the Payment REST API with the help of Postman, you need to prepare the following:

* Download and install latest version of the Postman application (https://www.postman.com/)
* Create an Sandbox application on https://developer.firstdata.eu/ and retrieve an the combination of ``Application Key`` and ``Application Secret``

## Collection & Environment Files


Type | Filename | Version | Download | Last Update
---------|----------|---------
 Collection | ``IPP Payment API Gateway (v5.2)`` | 5.2 | [download](https://github.com/Fiserv-Developer/payments-api/blob/2.0.1/postman/ipp-payment-api.postman_collection.v5.2.json) | 04.11.2020
 Environment | ``IPP Payment API v1.0 [Sandbox]`` | 1.0 | [download](https://github.com/Fiserv-Developer/payments-api/blob/2.0.1/postman/ipp-payment-api.postman_environment.v1.0.json) | 04.11.2020

## Configuration Steps

1. Import both collection and environment files into Postman application (see https://learning.postman.com/docs/getting-started/importing-and-exporting-data/)

2. Update the environment variables ``apiAppKey`` and ``apiAppSecret`` according to your values of your created Sandbox application (see https://learning.postman.com/docs/sending-requests/managing-environments/)