#Enexure.Sql.Dynamic.Providers.TransactSql (alpha)

TSql Provider for Dynamic Sql

#How to use 

Once you've constructed your [query](https://github.com/enexure/Enexure.Sql.Dynamic) you need to use a provider to generate an IDbCommand. 

	// Get the DbCommand
	var command = Provider.GetCommand(query)

Then you can execute the command as per standard ADO.Net. The parameters will automatically be populated. 