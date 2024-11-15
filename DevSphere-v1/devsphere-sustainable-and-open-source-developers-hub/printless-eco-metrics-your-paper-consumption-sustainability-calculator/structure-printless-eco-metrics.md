---
description: >-
  PrintLess Eco Metrics is a project aimed at reducing paper consumption and
  promoting eco-friendly practices. Here are some critical aspects of the
  project:
icon: rectangle-code
cover: >-
  ../../.gitbook/assets/A-1-1-Designer - 2024-11-09T183609.247-PrintLess Eco
  Metrics--myherb-app-01.jpeg
coverY: 122
---

# Structure - PrintLess Eco Metrics

* **SaaS Pricing Calculator**: Assists in determining the pricing for software-as-a-service products, potentially incorporating sustainability metrics.

{% hint style="success" %}
The project emphasizes the importance of sustainability in everyday practices and offers practical tools to help individuals and businesses make more eco-friendly choices.
{% endhint %}

{% code title="PrintLess Eco Metrics" overflow="wrap" lineNumbers="true" %}
```markup
'use client'

import { useState, useEffect } from 'react'
import { Card, CardContent, CardFooter, CardHeader, CardTitle, CardDescription } from "@/components/ui/card"
import { Label } from "@/components/ui/label"
import { Slider } from "@/components/ui/slider"
import { Input } from "@/components/ui/input"
import { Button } from "@/components/ui/button"
import { Tabs, TabsContent, TabsList, TabsTrigger } from "@/components/ui/tabs"
import { Leaf, Droplets, Package, DollarSign } from 'lucide-react'

export default function SustainabilityCalculator() {
  const [monthlyConsumption, setMonthlyConsumption] = useState(100)
  const [customInput, setCustomInput] = useState('')
  const [targetSavings, setTargetSavings] = useState('')
  const [calculatedSheets, setCalculatedSheets] = useState(0)

  // Sustainability metrics (example values, adjust as needed)
  const packagingSavedPerSheet = 0.005 // grams of packaging saved per A4 sheet
  const carbonSavedPerSheet = 0.01 // kg of CO2 saved per A4 sheet
  const waterSavedPerSheet = 10 // liters of water saved per A4 sheet
  const costSavingsPerSheet = 0.02 // dollars saved per A4 sheet due to reduced paper use and efficiency

  useEffect(() => {
    if (customInput !== '') {
      setMonthlyConsumption(Number(customInput))
    }
  }, [customInput])

  useEffect(() => {
    if (targetSavings !== '') {
      setCalculatedSheets(Math.ceil(Number(targetSavings) / costSavingsPerSheet))
    }
  }, [targetSavings])

  const packagingSaved = monthlyConsumption * packagingSavedPerSheet
  const carbonSaved = monthlyConsumption * carbonSavedPerSheet
  const waterSaved = monthlyConsumption * waterSavedPerSheet
  const totalSavings = monthlyConsumption * costSavingsPerSheet

  return (
    <Card className="w-full max-w-2xl mx-auto shadow-lg bg-gradient-to-br from-green-50 to-blue-50 dark:from-green-900 dark:to-blue-900">
      <CardHeader className="space-y-2">
        <CardTitle className="text-3xl font-bold text-green-700 dark:text-green-300">PrintLess Eco Metrics</CardTitle>
        <CardDescription className="text-lg font-semibold text-blue-600 dark:text-blue-300">
          Paper Consumption Sustainability Calculator
        </CardDescription>
      </CardHeader>
      <CardContent className="space-y-6">
        <Tabs defaultValue="sheets" className="w-full">
          <TabsList className="grid w-full grid-cols-2">
            <TabsTrigger value="sheets">Calculate by Sheets</TabsTrigger>
            <TabsTrigger value="savings">Calculate by Savings</TabsTrigger>
          </TabsList>
          <TabsContent value="sheets" className="space-y-4">
            <div className="space-y-2">
              <Label htmlFor="consumption" className="text-lg font-medium">Monthly Paper Consumption (A4 sheets)</Label>
              <div className="flex items-center space-x-2">
                <Slider
                  id="consumption"
                  min={0}
                  max={500}
                  step={1}
                  value={[monthlyConsumption]}
                  onValueChange={(value) => {
                    setMonthlyConsumption(value[0])
                    setCustomInput(value[0].toString())
                  }}
                  className="flex-grow"
                />
                <Input
                  type="number"
                  value={customInput}
                  onChange={(e) => setCustomInput(e.target.value)}
                  className="w-20"
                />
              </div>
              <div className="text-sm text-muted-foreground text-center">{monthlyConsumption} A4 sheets</div>
            </div>
          </TabsContent>
          <TabsContent value="savings" className="space-y-4">
            <div className="space-y-2">
              <Label htmlFor="targetSavings" className="text-lg font-medium">Target Cost Savings ($)</Label>
              <div className="flex items-center space-x-2">
                <Input
                  id="targetSavings"
                  type="number"
                  value={targetSavings}
                  onChange={(e) => setTargetSavings(e.target.value)}
                  className="flex-grow"
                />
                <Button onClick={() => setMonthlyConsumption(calculatedSheets)}>Calculate</Button>
              </div>
              <div className="text-sm text-muted-foreground text-center">
                {calculatedSheets} A4 sheets required to save ${targetSavings}
              </div>
            </div>
          </TabsContent>
        </Tabs>

        <div className="grid grid-cols-2 md:grid-cols-4 gap-4">
          <SavingsCard
            icon={<Package className="h-8 w-8 text-green-500" />}
            title="Packaging Saved"
            value={packagingSaved.toFixed(2)}
            unit="grams"
          />
          <SavingsCard
            icon={<Leaf className="h-8 w-8 text-green-500" />}
            title="Carbon Reduced"
            value={carbonSaved.toFixed(2)}
            unit="kg CO₂"
          />
          <SavingsCard
            icon={<Droplets className="h-8 w-8 text-blue-500" />}
            title="Water Saved"
            value={waterSaved.toFixed(2)}
            unit="liters"
          />
          <SavingsCard
            icon={<DollarSign className="h-8 w-8 text-green-500" />}
            title="Cost Savings"
            value={totalSavings.toFixed(2)}
            unit="$"
          />
        </div>
      </CardContent>
      <CardFooter className="flex-col items-start space-y-2">
        <div className="text-lg font-semibold text-green-700 dark:text-green-300">Your Environmental Impact</div>
        <p className="text-sm text-muted-foreground">
          By reducing paper consumption, you're not only saving money but also contributing to a more sustainable future!
          Every sheet counts towards a greener planet.
        </p>
        <div className="w-full text-right">
          <a href="https://myherb.co.il" target="_blank" rel="noopener noreferrer" className="text-xs text-muted-foreground hover:underline">
            Powered by myHerb.co.il
          </a>
        </div>
      </CardFooter>
    </Card>
  )
}

function SavingsCard({ icon, title, value, unit }) {
  return (
    <div className="bg-white dark:bg-gray-800 rounded-lg p-4 text-center shadow transition-all hover:shadow-md">
      <div className="flex justify-center mb-2">{icon}</div>
      <h3 className="text-sm font-medium mb-1">{title}</h3>
      <p className="text-lg font-semibold">
        {value} <span className="text-sm font-normal">{unit}</span>
      </p>
    </div>
  )
}
```
{% endcode %}

Latest Version: v0.13[^1]

[^1]: Application Version
