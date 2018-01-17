# 实际开发当中遇到的疑难问题
1.在使用NSNumberFormatter类格式化字符串时发现iOS10下没问题，升级到iOS 11 发现程序崩溃。当时的代码如下

+ (NSNumber *)stringToNumber:(id)string andFieldsName:(NSString *) fieldsNane{
    if ([TSSGDJsonToDic objectIsValid:string]) {
        NSNumberFormatter *numberFormatter = [[[NSNumberFormatter alloc] init]autorelease];
        
        [numberFormatter setNumberStyle:NSNumberFormatterDecimalStyle];
        
        if ([numberFormatter numberFromString:string])
        {
            return [numberFormatter numberFromString:string];
        }else{
            FIELD_FORMAT_ERROR(fieldsNane);
        }
    }
    return nil;
}

Answer：在使用NSNumberFormatter格式化字符串带小数点的数字时需要设置地区代码改动成这样就好了

+ (NSNumber *)stringToNumber:(id)string andFieldsName:(NSString *)fieldsNane {
    if ([TSSGDJsonToDic objectIsValid:string]) {
        NSNumberFormatter *numberFormatter = [[[NSNumberFormatter alloc] init]autorelease];
        numberFormatter.locale = [[NSLocale alloc] initWithLocaleIdentifier:@"zh_CN"];//加入设备的地区
        [numberFormatter setNumberStyle:NSNumberFormatterDecimalStyle];
        
        if ([numberFormatter numberFromString:string])
        {
            return [numberFormatter numberFromString:string];
        } else {
            FIELD_FORMAT_ERROR(fieldsNane);
        }
    }
    return nil;
}