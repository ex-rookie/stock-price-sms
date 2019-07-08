import json

def lambda_handler(event, context):
    
    from botocore.vendored import requests
    import boto3
    import os
    
    #Define ENV variables
    company_code = os.environ['COMPANY_CODE']
    phone_number = os.environ['PHONE_NUMBER']
    sender_name = os.environ['SENDER_NAME']
    exchange_api_key = os.environ['EXCHANGE_API_KEY']
    
    #Get share price data and convert to JSON for dict operations
    symbols = {'symbols': company_code}
    share_data = requests.get('https://query1.finance.yahoo.com/v7/finance/quote?lang=en-US&region=US&corsDomain=finance.yahoo.com&fields=regularMarketPrice', params=symbols)
    pricedata = share_data.json()
    
    #Get exchange-rate list and convert to JSON for dict operations
    apikey = {'app_id': exchange_api_key}
    curr_exchange = requests.get('https://openexchangerates.org/api/latest.json', params=apikey)
    exdata = curr_exchange.json()
    
    #Extract the USD price and AUD exchange-rate from the JSONs and calculate the price in AUD
    usdprice = pricedata["quoteResponse"]['result'][0]['regularMarketPrice']
    exrate = exdata['rates']['AUD']
    shareprice = str(round(usdprice * exrate, 2))

    #SMS the price
    client = boto3.client('sns')
    client.publish(PhoneNumber=phone_number, Message=shareprice, MessageAttributes={'AWS.SNS.SMS.SenderID': {'DataType': 'String', 'StringValue': sender_name}})