<?php
   /**
    *
    * Template Name: Search Results - Family
    *
    * The template for displaying content from pagebuilder.
    *
    * This is the template that displays pages without title in fullwidth layout. Suitable for use with Pagebuilder.
    *
    * @link https://codex.wordpress.org/Template_Hierarchy
    *
    * @package PesaBazaar
    */
   
   get_header();
   get_template_part( 'template-parts/family/search-banner');
   get_template_part( 'template-parts/family/search-radio');
   get_template_part( 'template-parts/family/search-results');
   get_template_part( 'template-parts/cta');
   get_template_part( 'template-parts/secondary-footer');
   get_footer();
   ?>
<script>
   var app = new Vue({
    el: "#page",
    data: {
        grouping: null,
        models: [],
        comparisondata: [],
        motorbenefits: [],
        motorexcesses: [],
        motorimportants: [],
   free_benefits: [],
   additional_benefits: [],
        step: 1,
        selectedCompanies: '',
   sortOrder: 'asc', // Default sorting order
		grandtotaldisplay: null,
   selectedOptions: [],
        submitting: true,
   detailState: {},
   contentState: {},
        lead: null,
        showBenefits: false,
   showMedicalDiv: false,
    showButton: true,
        showMoreDetails: false,
   showOptionalBenefits: false,
   addedOutPatient: null,
   fullSelectedBenefits: null,
   totalOutpatient: 0,
   showFeatures: false,
        detailsVisible: true,
		selectedResult: null,
		grandtotaldisplay: null,
		addedOutPatient: null,
		fullSelectedBenefits: null,
        res: {
            action: "getFamilyMedicalResults",
            id: null,
        },
   checkingaditional: null
    },
    created() {
        this.showBenefits = false;
        this.showMoreDetails = false; // Set showContent to false initially
    },
   computed: {
   total() {
   return this.selectedOptions.reduce((acc, cur) => acc + Number(cur), 0);
   },
	   promotedItems() {
                return this.comparisondata.filter(item => item.aditional.promoted == "Yes");
            },
	    sortedItems() {
		  // Create a copy of the items array to avoid mutating the original array
		  const sortedItems = [...this.comparisondata];
		  // Sort the array based on the price and sortOrder
		  sortedItems.sort((a, b) => {
			const priceA = a.data.newprice;
			const priceB = b.data.newprice;
			      console.log('priceA:', priceA, 'priceB:', priceB);

			// Adjust the sorting order based on the sortOrder variable
			if (this.sortOrder === 'asc') {
			  return priceA - priceB;
			} else {
			  return priceB - priceA;
			}
		  });
		  return sortedItems.filter(item => item.aditional.promoted == "No");
			 console.log('Sorted and filtered items:',sortedItems()); 
		},
   },
   
    methods: {
   sortByPrice(order) {
      this.sortOrder = order;
    },
   toggleFeatureList() {
      this.showFeatures = !this.showFeatures;
    },
   ageCalculator(ageBrackets, dateOfBirth, spouseDOB, numberOfChildren, perPersonOrShared, age_bracket_family, outpatient_cover_shared) {
   var currentDate = new Date();
   var birthDate = new Date(dateOfBirth);
   var age = currentDate.getFullYear() - birthDate.getFullYear();
   
   // Spouse birthday
   var spouseBirthDate = new Date(spouseDOB);
   var spouseAge = currentDate.getFullYear() - spouseBirthDate.getFullYear();
   
   
   // Children number
   var numberOfChildren = parseInt(numberOfChildren);
    var numberOfFamilyMembers = spouseAge !== null ? 2 : 1;
    var numberOfFamilyMembers = numberOfFamilyMembers + parseInt(numberOfChildren);
   
   var finalCost = 0;
   
   if (perPersonOrShared === 'perperson') {
        // Loop through the age brackets
        for (var i = 0; i < ageBrackets.length; i++) {
          var bracket = ageBrackets[i];
          if (age <= parseInt(bracket.age_bracket)) {
              // Calculate total cost for the main person, spouse, and children
              var principal = parseFloat(bracket.principal);
              var spouseCost = parseFloat(bracket.spouse);
              var childrenCost = numberOfChildren * parseFloat(bracket.children);
   
              var totalCost = principal + spouseCost + childrenCost;
   
            
   
              // Calculate the final cost including tax and stamp duty
              finalCost =  totalCost
   
              return finalCost;
          }
      }
   } else if (perPersonOrShared === 'shared') {
    //loop through the age_bracket_family
    for (var i = 0; i < age_bracket_family.length; i++) {
      var bracket = age_bracket_family[i];
      if (age <= parseInt(bracket.age_bracket) && numberOfFamilyMembers === parseInt(bracket.number_of_children)) {
        // Calculate total cost for the main person, spouse, and children
        var principal = parseFloat(bracket.inpatient);
   
        var totalCost = principal ;
   
   
        // Calculate the final cost including tax and stamp duty
        finalCost =  totalCost 
   
        return finalCost;
      }
    }
   }
   return finalCost;
   },
   
   ageCalculatorWithTax(ageBrackets, dateOfBirth, tax_rate, stamp_duty, spouseDOB, numberOfChildren, perPersonOrShared, age_bracket_family, outpatient_cover_shared, data) {
   var currentDate = new Date();
   var birthDate = new Date(dateOfBirth);
   var age = currentDate.getFullYear() - birthDate.getFullYear();
   
   // Spouse birthday
   var spouseBirthDate = new Date(spouseDOB);
   var spouseAge = currentDate.getFullYear() - spouseBirthDate.getFullYear();
   
   // Children number
   var numberOfChildren = parseInt(numberOfChildren);
   var numberOfFamilyMembers = spouseAge !== null ? 2 : 1;
    var numberOfFamilyMembers = numberOfFamilyMembers + parseInt(numberOfChildren);
   var finalCost = 0;
   
   if (perPersonOrShared === 'perperson') {
        // Loop through the age brackets
        for (var i = 0; i < ageBrackets.length; i++) {
          var bracket = ageBrackets[i];
          if (age <= parseInt(bracket.age_bracket)) {
              // Calculate total cost for the main person, spouse, and children
              var principal = parseFloat(bracket.principal);
              var spouseCost = parseFloat(bracket.spouse);
              var childrenCost = numberOfChildren * parseFloat(bracket.children);
   
              var totalCost = principal + spouseCost + childrenCost;
   
              // Calculate tax
              var tax = totalCost * parseFloat(tax_rate) / 100;
   
              // Calculate the final cost including tax and stamp duty
              finalCost = tax + totalCost + parseFloat(stamp_duty);
     
     // Check if data is defined and has 'newprice' property
   if (data && typeof data === 'object') {
    // Check if 'newprice' property exists in data
    if (!data.hasOwnProperty('newprice')) {
        data.newprice = 0; // Set a default value if 'newprice' doesn't exist
    }
     
     var totalString = finalCost
     
     data.newprice = parseInt(totalString);
   
              return finalCost;
          } }
      }
   } else if (perPersonOrShared === 'shared') {
    //loop through the age_bracket_family
    for (var i = 0; i < age_bracket_family.length; i++) {
      var bracket = age_bracket_family[i];
      if (age <= parseInt(bracket.age_bracket) && numberOfFamilyMembers === parseInt(bracket.number_of_children)) {
        // Calculate total cost for the main person, spouse, and children
        var principal = parseFloat(bracket.inpatient);
   
        var totalCost = principal ;
   
        // Calculate tax
        var tax = totalCost * parseFloat(tax_rate) / 100;
   
        // Calculate the final cost including tax and stamp duty
        finalCost = tax + totalCost + parseFloat(stamp_duty);
    
    // Check if data is defined and has 'newprice' property
   if (data && typeof data === 'object') {
    // Check if 'newprice' property exists in data
    if (!data.hasOwnProperty('newprice')) {
        data.newprice = 0; // Set a default value if 'newprice' doesn't exist
    }
    
   var totalString = finalCost
     
   data.newprice = parseInt(totalString);
   
        return finalCost;
      } }
    }
   }
   return finalCost;
   },
   ageOutpatienCalculator(ageBrackets, dateOfBirth, numberOfChildren, spouseDOB){
     var currentDate = new Date();
     var birthDate = new Date(dateOfBirth);
     var age = currentDate.getFullYear() - birthDate.getFullYear();
   var spouseBirthDate = new Date(spouseDOB);
    var spouseAge = currentDate.getFullYear() - spouseBirthDate.getFullYear();
     var finalPrincipal = [];
   var numberOfChildren 
   
     // Loop through the age brackets
     for (var i = 0; i < ageBrackets.length; i++) {
   var bracket = ageBrackets[i];
   if (age >= parseInt(bracket.age_bracket_minimum) && age <= parseInt(bracket.age_bracket_maximum)) {
    // Return the principal based on the age bracket
    finalPrincipal.push(ageBrackets[i]);
   		}
     }
   return finalPrincipal;
   console.log(finalPrincipal);
   },
   
   ageOutpatientCalculator(com, checkingaditional, ageBrackets, dateOfBirth, spouseDOB, numberOfChildren, outpatient_cover_shared, perPersonOrShared) {
   console.log(checkingaditional);
   console.log(outpatient_cover_shared);
   console.log(perPersonOrShared);
   
   
   var currentDate = new Date();
   var birthDate = new Date(dateOfBirth);
   var age = currentDate.getFullYear() - birthDate.getFullYear();
   
   
   // Spouse birthday
   var spouseBirthDate = new Date(spouseDOB);
   var spouseAge = currentDate.getFullYear() - spouseBirthDate.getFullYear();
   
   // Children number
   var numberOfChildren = parseInt(numberOfChildren);
   var finalPrincipal = [];
   var finalSpouse = [];
   var finalChildren = [];
   if (perPersonOrShared === 'perperson') {
   console.log ('You are on per-person');
    // Loop through the age brackets
    var totalDisplayPremium = 0;
   if(checkingaditional === com.data.id){
    for (var i = 0; i < ageBrackets.length; i++) {
     var bracket = ageBrackets[i];
     if (age >= parseInt(bracket.age_bracket_minimum) && age <= parseInt(bracket.age_bracket_maximum)) {
   	// Return the principal based on the age bracket
   		finalPrincipal.push(ageBrackets[i]);
   // 					finalPrincipal = parseInt(ageBrackets[i].outpatient_cover_cost);
   		  totalDisplayPremium = totalDisplayPremium + parseInt(ageBrackets[i].outpatient_cover_cost);
   	  }
   	if (spouseAge >= parseInt(bracket.age_bracket_minimum) && spouseAge <= parseInt(bracket.age_bracket_maximum)) {
   	  // Return the spouse coverage based on the age bracket
   	  finalSpouse = parseInt(ageBrackets[i].outpatient_cover_cost);
   		totalDisplayPremium = totalDisplayPremium + parseInt(ageBrackets[i].outpatient_cover_cost);
   	}
   
   	var childAge = 18
   	if (childAge >= parseInt(bracket.age_bracket_minimum) && childAge <= parseInt(bracket.age_bracket_maximum)) {
     // Return the spouse coverage based on the age bracket
     finalChildren = parseInt(ageBrackets[i].outpatient_cover_cost) * numberOfChildren;
   		totalDisplayPremium = totalDisplayPremium + finalChildren;
   }
   }
   
   console.log("Per Person Outpatient Covers (Principal):", finalPrincipal);
    console.log("Per Person Outpatient Cover (Spouse):", finalSpouse);
    console.log("Outpatient Cover for Children (0-18):", finalChildren);
    console.log("Total:", totalDisplayPremium);
   
   var benefits = [{
   'principal': finalPrincipal,
      		'spouse': finalSpouse,
   'children': finalChildren,
   'totalDisplayPremium': totalDisplayPremium
   }];
   
    return benefits;
    
    
   }
   
   
   
   } else if (perPersonOrShared === 'shared') {
    console.log ('You are on shared');
    // Loop through the outpatient_cover_shared
    for (var i = 0; i < outpatient_cover_shared.length; i++) {
   console.log (outpatient_cover_shared.length);
      var bracket = outpatient_cover_shared[i];
   console.log("Bracket:", bracket);
   console.log("Aaage:", age);
   
   
      if ((age >= parseInt(bracket.age_bracket_minimum)) && (age <= parseInt(bracket.age_bracket_maximum))) {
        // Return the principal based on the age bracket
        finalPrincipal.push(bracket);
    	console.log("Aaage Mini:", bracket.age_bracket_minimum);
   
   console.log("Aaage Max:", bracket.age_bracket_maximum);
    
      }
    }
   
    console.log("Shared Outpatient Covers:", finalPrincipal); // Log the results
    return finalPrincipal;
   }
   },
   
   formattedCoverLimit() {
   return this.formatCurrency(this.lead.cover_limit);
   },
   formatCurrency(value) {
   var num_parts = parseFloat(value).toFixed(0).toString().split(".");
    num_parts[0] = num_parts[0].replace(/\B(?=(\d{3})+(?!\d))/g, ",");
    return num_parts.join(".");
   },
   formatPercentage(value) {
   return value + "%";
   },
   outpatientCover(val, data, fulldata) {
	   this.addedOutPatient = val;
	   console.log("Added OutPatient:", this.addedOutPatient);
	   this.showOptionalBenefits = true;
	   this.totalOutpatient = val;
	   var container = document.getElementById('666' + data.id);
	   var nonTaxcontainer = document.getElementById('444' + data.id).value;
	   var arr = nonTaxcontainer.split(',');
	   var rate = arr.join('');

			var grandTotal = 0;
	   var grand = parseFloat(val) + parseFloat(rate) + parseFloat(fulldata.stamp_duty);
	   grandTotal = ((parseFloat(fulldata.tax_rate) / 100) * grand) + grand;
			container.innerHTML = this.formatCurrency(grandTotal);
	   this.grandtotaldisplay = grandTotal;
    
   },
   removeOutpatientCover(data) {
	   this.addedOutPatient = null;
   this.totalOutpatient = null;
   this.showOptionalBenefits = false;
   
   var container = document.getElementById('666' + data.id);
   var fallBackContainer = document.getElementById('555' + data.id).value;
   var arrFallBac = fallBackContainer.split(',');
   var rateFine = arrFallBac.join('');
   // Update the value of 'container' to use 'fallBackContainer'
   container.innerHTML = this.formatCurrency(rateFine);
	   
	   const existingData = localStorage.getItem("myBazaarCartMedical_" + data.id);

	   if (undefined !== existingData && existingData != null) {
		   var res = JSON.parse(existingData);

		   var data1 = res.data;
		   var total1 = res.total;
		   var benefit1 = [];
		   var client1 = res.client;

		   const selectedBenefits = {
			   data: data1,
			   total: total1,
			   benefit: benefit1,
			   client: client1
		   };

		   localStorage.setItem("myBazaarCartMedical_" + data.id, JSON.stringify(selectedBenefits));
	   }

	   this.fullSelectedBenefits = null;
	   this.grandtotaldisplay = rateFine;
	   const button = document.getElementById('benefits-btn' + data);
	   if (button) {
		// Hide the button by setting display to 'none'
		button.style.display = 'block';
	   }
   },
   
   
   selectedData(val, data, client, fulldata) {
    var container = document.getElementById('666' + data.id);
    var nonTaxcontainer = document.getElementById('444' + data.id).value;
    var fallBackContainer = document.getElementById('555' + data.id).value;
    var arr = nonTaxcontainer.split(',');
    var rate = arr.join('');
   //    console.log('Total Outpatient:', this.totalOutpatient);
    var arrFallBac = fallBackContainer.split(',');
    var rateFine = arrFallBac.join('');
    var totalOutpatient = parseFloat(this.totalOutpatient);
   
   //     console.log("data:", data);
   //     console.log("client:", client);
   //     console.log("container:", container);
   //     console.log("nonTaxcontainer:", nonTaxcontainer);
   //     console.log("fallBackContainer:", fallBackContainer);
   //     console.log("arr:", arr);
   //     console.log("rate:", rate);
   //     console.log("arrFallBac:", arrFallBac);
   //     console.log("rateFine:", rateFine);
   
    const existingData = localStorage.getItem("myBazaarCartMedical_" + data.id); // Unique storage key
   
   //     console.log("existingData:", existingData);
   
    if (undefined !== existingData && existingData != null) {
        var res = JSON.parse(existingData);
   
        if (res.data.id == data.id) {
            var storageBenefit = res.benefit;
   
            var filteredResult = storageBenefit.filter((br) => {
                return (parseFloat(br.cost) == parseFloat(val.cost)) && (br.content == val.content);
            });
   
            if (filteredResult.length != 0 && filteredResult[0] != null && filteredResult[0] != undefined) {
   
                var removingFilter = storageBenefit.filter((rpt) => {
                    return (rpt.content != filteredResult[0].content);
                });
   
                storageBenefit = removingFilter;
   
                var benefitsTotal = 0;
                var fullTotal = 0;
                var totalTax = 0;
                var grandTotal = 0;
                var nontaxbnefit = 0;
   
                storageBenefit.forEach(element => {
                    if (parseFloat(element.rate) <= 0) {
                        nontaxbnefit = nontaxbnefit + parseFloat(element.cost);
                    } else {
                        benefitsTotal = benefitsTotal + parseFloat(element.cost);
                    }
                });
   
                fullTotal = nontaxbnefit + parseFloat(rate) + parseFloat(fulldata.stamp_duty) + totalOutpatient;
   
                grandTotal = ((parseFloat(fulldata.tax_rate) / 100) * fullTotal) + fullTotal;
   
                container.innerHTML = this.formatCurrency(grandTotal);
   
            } else {
                storageBenefit.push(val);
   
                var benefitsTotal = 0;
                var fullTotal = 0;
                var totalTax = 0;
                var grandTotal = 0;
                var nontaxbnefit = 0;
   
                storageBenefit.forEach(element => {
                    if (parseFloat(element.rate) <= 0) {
                        nontaxbnefit = nontaxbnefit + parseFloat(element.cost);
                    } else {
                        benefitsTotal = benefitsTotal + parseFloat(element.cost);
                    }
                });
   
                fullTotal = nontaxbnefit + parseFloat(rate) + parseFloat(fulldata.stamp_duty) + totalOutpatient;
   
                grandTotal = ((parseFloat(fulldata.tax_rate) / 100) * fullTotal) + fullTotal;
				
				this.grandtotaldisplay = grandTotal;
				
				this.fullSelectedBenefits = storageBenefit;
   
                container.innerHTML = this.formatCurrency(grandTotal);
            }
   
            const selectedBenefits = {
                data: data,
                total: grandTotal,
                benefit: storageBenefit,
                client: client
            };
   
            localStorage.setItem("myBazaarCartMedical_" + data.id, JSON.stringify(selectedBenefits));
   
        } else {
            var benefitsTotal = 0;
            var fullTotal = 0;
            var totalTax = 0;
            var grandTotal = 0;
            var nontaxbnefit = 0;
   
            if (parseFloat(val.rate) <= 0) {
                var grandT = parseFloat(val.cost) + parseFloat(rateFine) + parseFloat(fulldata.stamp_duty) + totalOutpatient;
   	
   	grandTotal = ((parseFloat(fulldata.tax_rate) / 100) * grandT) + grandT;
            } else {
                benefitsTotal = parseFloat(val.cost) + parseFloat(rate) + totalOutpatient;
   
                totalTax = (parseFloat(fulldata.tax_rate) / 100) * benefitsTotal;
   
                grandTotal = benefitsTotal + totalTax;
				
				this.grandtotaldisplay = grandTotal;
				
				this.fullSelectedBenefits = storageBenefit;
            }
   
            container.innerHTML = this.formatCurrency(grandTotal);
   
            var benefitdata = [val];
   
            const selectedBenefits = {
                data: data,
                total: grandTotal,
                benefit: benefitdata,
                client: client
            };
   
            localStorage.setItem("myBazaarCartMedical_" + data.id, JSON.stringify(selectedBenefits)); // Unique storage key
        }
   
    } else {
        var benefitsTotal = 0;
        var fullTotal = 0;
        var totalTax = 0;
        var grandTotal = 0;
        var nontaxbnefit = 0;
   
        if (parseFloat(val.rate) <= 0) {
            var grand = parseFloat(val.cost) + parseFloat(rate) + parseFloat(fulldata.stamp_duty) + totalOutpatient;
   
   grandTotal = ((parseFloat(fulldata.tax_rate) / 100) * grand) + grand;
   
        } else {
            benefitsTotal = parseFloat(val.cost) + parseFloat(rate) + totalOutpatient;
   
            totalTax = (parseFloat(val.rate) / 100) * benefitsTotal;
   
            grandTotal = benefitsTotal + totalTax;
        }
   		this.grandtotaldisplay = grandTotal;
        container.innerHTML = this.formatCurrency(grandTotal);
   
        var benefitdata = [val];
   
        const selectedBenefits = {
            data: data,
            total: grandTotal,
            benefit: benefitdata,
            client: client
        };
   
        localStorage.setItem("myBazaarCartMedical_" + data.id, JSON.stringify(selectedBenefits)); // Unique storage key
    }
   },
   
   
   
   redirectitosuccesspage(){
   let url = '<?php echo site_url();?>/thank-you/?product=medical';
   window.location.replace(url);
   },
        async getresults(id) {
            console.log(id);
            this.res.id = id;
            var postdata = this.res;
            const formData = new FormData();
            for (var key in postdata) {
                formData.append(key, postdata[key]);
            }
            this.submitting = true;
            try {
                console.log(formData);
                fetch("<?php echo admin_url('admin-ajax.php'); ?>", {
                    method: "POST",
                    body: formData,
                })
                    .then((res) => {
                        return res.json();
                    })
                    .then((data) => {
                        this.comparisondata = data;
                    })
                    .catch((err) => console.error(err));
   
                this.submitting = false;
            } catch (e) {
                console.log("error", e);
   
                this.submitting = false;
                return;
            }
        },
        toggleBenefits(data) {
   console.log(data);
   
   
   console.log(this.checkingaditional);
   if(data == this.checkingaditional){
      this.checkingaditional == null
   	this.showMoreDetails = false;
   }else if(this.checkingaditional != null && this.checkingaditional != data){
   	this.checkingaditional = data
   	this.showBenefits = true;
   	this.showMoreDetails = false;
   }else if(this.checkingaditional == null){
   	this.checkingaditional = data
   	this.showBenefits = !this.showBenefits;
   	this.showMoreDetails = false;
   }else if(this.checkingaditional === data){
   	this.checkingaditional = null;
   	this.showBenefits = false;
   	this.showMoreDetails = false;
   }
   const button = document.getElementById('benefits-btn' + data);
   if (button) {
    // Hide the button by setting display to 'none'
    button.style.display = 'none';
   }
        },
        
   
   // 		isContentOpen(id) {
   // 		  return this.contentState[id] || false;
   // 		},
   // 		toggleBenefits(id) {	  
   // 		  this.$set(this.contentState, id, !this.isContentOpen(id));
   // 		},
   isDetailOpen(id){
   return this.detailState[id] || false;
   },
        toggleDetails(id) {
           console.log(id);
           this.$set(this.detailState, id, !this.isDetailOpen(id));
        },
        hideDetails() {
            this.showMoreDetails = false;
            this.detailsVisible = true;
        },
      
    },
    mounted() {
        let uri = window.location.href.split("?");
        if (uri.length >= 2) {
            let vars = uri[1].split("&");
            let getVars = {};
            let tmp = "";
            vars.forEach(function (v) {
                tmp = v.split("=");
                if (tmp.length == 2) getVars[tmp[0]] = tmp[1];
            });
            this.res.id = getVars.id;
            //                     if (getVars.id != null && getVars.id == '') {
            //                         this.getresults(getVars.id);
   
            var postdata = this.res;
            const formData = new FormData();
            for (var key in postdata) {
                formData.append(key, postdata[key]);
            }
            this.submitting = true;
            try {
                fetch("<?php echo admin_url('admin-ajax.php'); ?>", {
                    method: "POST",
                    body: formData,
                })
                    .then((res) => {
                        return res.json();
                    })
                    .then((data) => {
                        console.log(data);
                        this.comparisondata = data.data;
                        this.lead = data.clientdata;
                    })
                    .catch((err) => console.error(err));
   
                this.submitting = false;
            } catch (e) {
                console.log("error", e);
   
                this.submitting = false;
                return;
            }
            //                     }
        } else {
            var url = "https://pesabazaar.belva.co.ke/api/setup";
            fetch(url)
                .then((response) => {
                    return response.json();
                })
                .then((data) => {
                    this.grouping = data;
                    this.models = data.brands;
                    this.use = data.motortypes;
                });
        }
    },
   });
   
</script>
